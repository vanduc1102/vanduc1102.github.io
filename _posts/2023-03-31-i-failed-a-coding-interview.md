---
layout: post
title: Problem - A store to give change back in the most efficient way
date: 2023-03-29 06:14:27 +0700
categories: [java]
tags: [problem, challenge]
---

This is a challenge I failed during a recent technical interview.

## Problem

Write a solution for a store to give change back in the most efficient way. Assuming you have `$10, $5, $2 and $1 coins` and that the most optimal way to give change back is by always giving the **LEAST** number of coins possible, how would you calculate which coins to return?

- **Input**: The amount of coins available for each denomination (e.g., 5 coins of each) and the total change amount to return.

- **Output**: How many coins of each denomination to return (e.g., 1 of $2, 3 of $5...).

## First attempt

```java

public class LeastChangeGiveBackTests {

    Map<Integer, Integer> arrayToMap(Integer[][] coins) {
        Map<Integer, Integer> map = new TreeMap<>(Collections.reverseOrder());
        for (Integer[] coin : coins) {
            map.put(coin[0], coin[1]);
        }
        return map;
    }

    Map<Integer, Integer> getLeastChanges(
        Integer[][] input, Integer change
    ) {
        Map<Integer, Integer> coins = arrayToMap(input);
        Map<Integer, Integer> result = new TreeMap<>(Collections.reverseOrder());

        int sum = 0;
        for (Map.Entry<Integer, Integer> entry : coins.entrySet()) {
            sum += entry.getKey() * entry.getValue();
        }
        if (sum <= change) {
            return result;
        }

        for (Map.Entry<Integer, Integer> entry : coins.entrySet()) {
            int needUnit = change / entry.getKey();
            int remainder = change % entry.getKey();

            if (needUnit > 0 && needUnit <= entry.getValue()) {
                result.put(entry.getKey(), needUnit);
                remainder = change - needUnit * entry.getKey();
            } else if (needUnit > entry.getValue()) {
                result.put(entry.getKey(), entry.getValue());
                remainder = change - entry.getKey() * entry.getValue();
            }
            change = remainder;
            if (remainder == 0) {
                break;
            }

        }

        return result;
    }

    @Test
    void testChangeIsBiggerThan10() {
        assertEquals(arrayToMap(
                new Integer[][]{
                    new Integer[]{10, 2},
                    new Integer[]{5, 1},
                    new Integer[]{2, 2},
                }),
            getLeastChanges(
                new Integer[][]{
                    new Integer[]{10, 3},
                    new Integer[]{5, 3},
                    new Integer[]{2, 13},
                    new Integer[]{1, 13}
                }, 29)
        );
    }

    @Test
    void testChangeIsLess10() {
        assertEquals(
            arrayToMap(
                new Integer[][]{
                    new Integer[]{5, 1},
                    new Integer[]{2, 1},
                }), getLeastChanges(
                new Integer[][]{
                    new Integer[]{10, 3},
                    new Integer[]{5, 3},
                    new Integer[]{2, 13},
                    new Integer[]{1, 13}
                }, 7)
        );
    }

    @Test
    void testChangeIs1() {
        assertEquals(getLeastChanges(
            new Integer[][]{
                new Integer[]{10, 3},
                new Integer[]{5, 3},
                new Integer[]{2, 13},
                new Integer[]{1, 13}
            }, 1), arrayToMap(
            new Integer[][]{
                new Integer[]{1, 1},
            }
        ));
    }

    @Test
    void testChangeIsExceeded() {
        assertEquals(arrayToMap(
            new Integer[][]{
            }
        ), getLeastChanges(
            new Integer[][]{
                new Integer[]{10, 3},
                new Integer[]{5, 3},
                new Integer[]{2, 13},
                new Integer[]{1, 100}
            }, 300));
    }
}
```
