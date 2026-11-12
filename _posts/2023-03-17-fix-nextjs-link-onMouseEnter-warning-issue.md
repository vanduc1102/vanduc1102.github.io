---
layout: post
title: The legacy behavior requires onMouseEnter be set on the child of next/link
date: 2023-03-17 06:14:27 +0700
categories: [reactjs]
tags: [reactjs, antd]
---

I encountered an issue in a project using [Blitz.js](https://blitzjs.com/) (built on top of [Next.js](https://nextjs.org/docs/api-reference/next/link)) and [Ant Design](https://ant.design/components/button) as the UI framework.

I noticed the following warning in the browser console:

```
onMouseEnter was passed to <Link> with href of [object Object] but "legacyBehavior" was set. The legacy behavior requires onMouseEnter be set on the child of next/link
```

## 1. Problematic Code

```tsx
<Tooltip title="for dummies">
	<Link href={Routes.ShowProductPage({ slug: "for-dummies" })} passHref>
		<Button icon={<EyeOutlined />} type="link"></Button>
	</Link>
</Tooltip>
```

## 2. Solution

- Create a wrapper component integrating `next/link` with Ant Design's `Button`:

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

- Fix the issue by using the new component:

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
