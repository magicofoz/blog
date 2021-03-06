---
title: 用 ESLint 和 Prettier 写出高质量代码
subtitle: 当然你的代码本身不能是无意义的
date: 2017-12-11 21:37:05
tags:
  - javascript
  - eslint
  - prettier
---

[ESLint](https://github.com/eslint/eslint) 可以检测出你代码中潜在的问题，比如使用了某个变量却忘记了定义，而 [Prettier](https://github.com/prettier/prettier) 作为代码格式化工具，能够统一你或者你的团队的代码风格。

本文不会介绍这两种工具单独的使用方法，因为相关网络资料已经足够多了。这里我介绍一下怎么同时使用它们。

## 在 ESLint 中使用 eslint-plugin-prettier

__为什么使用这种方法: 希望格式化结果完全符合 Prettier 的要求。__

`eslint-plugin-prettier` 的工作原理是，对比格式化前和用 Prettier 格式化后的代码，有不一致的地方就会报错，然后你可以手动运行 `eslint --fix` 来修复。

不过 Prettier 的格式化很可能和你使用的 ESLint 配置冲突，比如你的 ESLint 设定的不使用分号，而 Prettier 默认使用分号，这时候你需要配置 Prettier 让它不使用分号。反之如果你想使用 Prettier 的规则而不是 ESLint 的，为防止 ESLint 报错，你需要使用 `eslint-config-prettier` 来关闭所有可能引起冲突的规则。

总结一下我的配置如下 `package.json`:

```json
{
  "scripts": {
    "lint": "eslint *.js"
  },
  "eslintConfig": {
    "extends": [
      "prettier"
    ],
    "plugins": [
      "prettier"
    ],
    "rules": {
      "prettier/prettier": "error"
    }
  },
  "prettier": {
    "singleQuote": true,
    "semi": false
  }
}
```

你可以使用大多数编辑器里 ESLint 插件提供的 `autoFixOnSave` 选项来在保存时自动格式化。

## 使用 prettier-eslint-cli

__为什么使用它: 希望格式化结果完全符合 ESLint 的要求。__

使用 ESLint 与 `eslint-plugin-prettier` 的结果是最终得到的代码是充分尊重 Prettier 的结果，而 `prettier-eslint-cli` 则是先执行 Prettier 然后再自动使用 `eslint --fix` 将与 ESLint 规则冲突的代码修正成 ESLint 想要的结果。这样其实引入 Prettier 不会影响你原有的设置。

总结一下我的配置如下 `package.json`:

```json
{
  "scripts": {
    // ESLint 只负责检查代码质量
    "lint": "eslint *.js",
    // 用 Prettier 格式化代码并自动 `eslint --fix `以便下次执行 `npm run lint` 时不会报错
    "format": "prettier-eslint --write *.js"
  },
  "eslintConfig": {
    "extends": "some-config"
  },
  "prettier": {
    "singleQuote": true,
    "semi": false
  }
}
```

除了命令行的 `npm run format`，同时你也可以使用编辑器里的 Prettier 插件，并启用 `eslintIntegration` 来在编辑器里自动格式化代码。
