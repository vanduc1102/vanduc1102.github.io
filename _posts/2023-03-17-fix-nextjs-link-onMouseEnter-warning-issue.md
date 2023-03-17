---
layout: post
title: The legacy behavior requires onMouseEnter be set on the child of next/link
date: 2023-03-17 06:14:27 +0700
categories: [reactjs]
tags: [reactjs, antd]
---

I have encountered an issue in my current project, I am using [blitzjs](https://blitzjs.com/), which is a full stack approach on top of [nextjs](https://nextjs.org/docs/api-reference/next/link), and [Ant.design](https://ant.design/components/button) as UI framework.

I have a WARNING in console:

onMouseEnter" was passed to <Link> with `href` of `[object Object]` but "legacyBehavior" was set. The legacy behavior requires onMouseEnter be set on the child of next/link

## 1. Code have issue

```tsx
<Tooltip title="for dummies">
	<Link href={Routes.ShowProductPage({ slug: "for-dummies" })} passHref>
		<Button icon={<EyeOutlined />} type="link"></Button>
	</Link>
</Tooltip>
```

## 2. Solution

- Create a new component for next/link and antd button

```tsx
import React from "react";
import Link, { LinkProps } from "next/link";
import { Button, type ButtonProps } from "antd";

type AntdButtonNextLinkProps = LinkProps & Omit<ButtonProps, "href">;

const AntdButtonNextLink = React.forwardRef(
	({ href, children, ...props }: AntdButtonNextLinkProps, ref: any) => {
		return (
			<Link href={href} passHref>
				<Button ref={ref} {...props} type="link">
					{children}
				</Button>
			</Link>
		);
	}
);
```

- Fix the issue by using the new component

```tsx
<Tooltip title="for dummies">
	<AntdButtonNextLink
		href={Routes.ShowProductPage({ slug: "for-dummies" })}
		icon={<EyeOutlined />}
	/>
</Tooltip>
```

### References:

1. [https://github.com/chakra-ui/chakra-ui/issues/6105](https://github.com/chakra-ui/chakra-ui/issues/6105)
