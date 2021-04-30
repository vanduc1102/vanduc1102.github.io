---
layout: post
title:  Jest common functions
date:   2020-10-20 00:14:27 +0700
categories: [javascript, jest, reactjs]
---

# Jest common functions
## Existence

`getBy*` and `getAllBy*` throw an error if nothing is found.

However, the equivalent methods `queryBy*` and `queryAllBy*` instead return `null` or `[]`:


```

expect(getAllByTestId('collection-share-button')).toHaveLength(1);
```

## None Existence

```
expect(() => getByText('your text')).toThrow('Unable to find an element');
```

[https://stackoverflow.com/questions/52783144/how-do-you-test-for-the-non-existence-of-an-element-using-jest-and-react-testing](https://stackoverflow.com/questions/52783144/how-do-you-test-for-the-non-existence-of-an-element-using-jest-and-react-testing)


## Expect on callback function

```js
it('should call onItemClick with actionId', () => {
    const handleItemClick = jest.fn();
    const { getByTestId } = render(
      withThemeProvider(
        <CollectionButtonGroup
          isFollowing={true}
          isOwnCollection={true}
          onButtonClick={handleItemClick}
        />
      )
    );

    fireEvent.click(getByTestId('collection-delete-button'));

    expect(handleItemClick).toHaveBeenCalledWith(
      CollectionButtonGroupAction.Delete
    );
  });
```
