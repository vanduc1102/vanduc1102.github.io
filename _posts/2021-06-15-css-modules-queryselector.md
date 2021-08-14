---
layout: post
title: CSS modules with CSS selectors
date:   2021-06-15 16:43:00 +0700
categories: [reactjs]
tags: [CSS, reactjs, CSSModules]
---

# How to use CSS selectors in CSS Modules
Because all classes will be hashed in CSS Modules, so you cant use class selector in your CSS

But you can still use tags, roles, psudo

Here is an example of the uses:

```
.Tab {
  > div:first-child {
    justify-content: flex-end;
    padding-right: 30px;
  }

  button {
    border-radius: initial;
  }

  button[aria-selected='true'] {
    border-bottom: 3px solid var(--wp-admin-theme-color);
  }

  button:focus {
    border-bottom: 3px solid var(--wp-admin-theme-color);
    box-shadow: initial !important;
  }
}

```
This is an example of https://developer.wordpress.org/block-editor/reference-guides/components/tab-panel/#development-guidelines


```
import { TabPanel } from '@wordpress/components';
import styles from 'index.module.scss';

const onSelect = ( tabName ) => {
    console.log( 'Selecting tab', tabName );
};

const MyTabPanel = () => (
    <TabPanel
        className={styles.Tab}
        activeClass="active-tab"
        onSelect={ onSelect }
        tabs={ [
            {
                name: 'tab1',
                title: 'Tab 1',
                className: 'tab-one',
            },
            {
                name: 'tab2',
                title: 'Tab 2',
                className: 'tab-two',
            },
        ] }
    >
        { ( tab ) => <p>{ tab.title }</p> }
    </TabPanel>
);

```
