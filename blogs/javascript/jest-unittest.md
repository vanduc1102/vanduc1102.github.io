# Jest common

## Existence

```

expect(getAllByTestId('collection-share-button')).toHaveLength(1);
```

## None Existence

```
expect(() => getByText('your text')).toThrow('Unable to find an element');
```

https://stackoverflow.com/questions/52783144/how-do-you-test-for-the-non-existence-of-an-element-using-jest-and-react-testing
