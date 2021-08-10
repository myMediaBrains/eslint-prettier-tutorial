# ESLint README

가독성 높은 코드를 작성하기 위한 모듈이다.

| no   | 구분     | 설명                                                         |
| ---- | -------- | ------------------------------------------------------------ |
| 1    | ESLint   | 일관된 코드 스타일을 작성하도록 문법적 오류나 안티 패턴을 **찾아준다.** |
|      |          | Airbnb, Google Style Guide 등 외부 스타일 가이드를 적용할 수도 있다. |
| 2    | Prettier | 정해진 코딩 스타일로 **변환**해주는 자바스크립트 라이브러이다. |

| no   | 구분            | 설명 |
| ---- | --------------- | ---- |
| 1    | .eslintrc.yaml  |      |
|      | .elintignore    |      |
| 2    | .prettierrc     |      |
|      | .prettierignore |      |

##  ESLint

| no   | 구분                      | 설명                                                         |
| ---- | ------------------------- | ------------------------------------------------------------ |
| 1    | $ yarn -W -D add eslint   | eslint 설치                                                  |
|      |                           | workspaces 까지 적용하려면, -W 옵션을 붙인다.                |
|      |                           | 만약, root 폴더에만 적용하려면, \--ignore-workspace-root-check  옵션을 사용한다. |
| 2    | $ yarn run eslint \--init | 여러 질문에 답을 하면, .eslintrc 가 자동 생성된다.           |

.eslintrc.json

```json
{
    "env": {
        "browser": true,
        "es2021": true
    },
    "extends": [
        "eslint:recommended",
        "plugin:react/recommended",
        "plugin:@typescript-eslint/recommended"
    ],
    "parser": "@typescript-eslint/parser",
    "parserOptions": {
        "ecmaFeatures": {
            "jsx": true
        },
        "ecmaVersion": 12,
        "sourceType": "module"
    },
    "plugins": [
        "react",
        "@typescript-eslint"
    ],
    "rules": {
    }
}
```

| no   | 구분                          | 설명                                           |
| ---- | ----------------------------- | ---------------------------------------------- |
| 3    | $ yarn run eslint yourfile.js | 어떤 파일에 대하여 eslint 를 테스트할 수 있다. |

## Configuring ESLint

| 구분                  | no         | 설명                                                         |
| --------------------- | ---------- | ------------------------------------------------------------ |
| ESLint configure 방법 | 1          | **(자바스크립트 comments)** 를 사용하여, configuration information을 파일에 직접 embed 하는 방법 |
|                       | 2          | **(자바스크립트, JSON, YAML 파일)**을 사용하여, 전체 디렉토리 및 서브디렉토리에 configuration information을 지정하는 방법 |
|                       |            | 1) .eslintrc.js, eslintrc.json, eslintrc.yaml, eslintrc.yml 의 별도 파일로 지정할 수 있다. |
|                       |            | 2) package.json 파일의 eslintConfig 속성으로 정의할 수도 있다. |
| **settings**          | definition | ESLint는 shared settings 추가하기를 지원한다.                |
|                       |            | plugins 는 settings 를 사용하여, 전체 rules 에서 share 되어져야 하는 information 을 지정할 수 있다. |

```json
{
  "settings": {
    "sharedData": "Hello"
  }
}
```

| cascading and hierarchy | 구분                        | 설명                                                         |
| ----------------------- | --------------------------- | ------------------------------------------------------------ |
| cascading               | precedences                 | 동일 디렉토리에 있는 .eslintrc 파일이 우선권을 갖는다.       |
|                         |                             | 그 다음, ESLint 는 root: true 또는 root 디렉토리에 도달할 때까지 디렉토리 구조에서 .eslintrc 파일들을 merge 한다. |
|                         |                             | ESLint는 root: true 가 발견되면, 더 이상 .eslintrc.* 를 찾지 않고 멈춘다. |
|                         | inline configuration        | 1) /\*eslint-disable*/ and /\*eslint-enable\*/               |
|                         |                             | 2) /\*global\*/                                              |
|                         |                             | 3) /\*eslint*/                                               |
|                         |                             | 4) /\*eslint-env*/                                           |
|                         | command line options        | 1) \--global                                                 |
|                         |                             | 2) \--rule                                                   |
|                         |                             | 3) \--env                                                    |
|                         |                             | 4) \-c, \--config                                            |
|                         | project-level configuration | 1) 동일 디렉토리에 있는 .eslintrc.* 또는 package.json        |
|                         |                             | 2) root 디렉토리 또는 “root”: true 가 발견될 때까지 ancestor 에 있는 .eslintrc.* 및 package.json 을 searching 한다. |



```json
{
  "root": true
}
```

| extends                           | 구분                                                   | 설명                                                         |
| --------------------------------- | ------------------------------------------------------ | ------------------------------------------------------------ |
| extends                           | definition                                             | 일단 어떤 configuration file 이 entended 되면, 또 다른 configuration 파일의 모든 특성을 상속받고, 모든 options를 수정할 수 있다. |
|                                   | 3가지 configuration                                    | (base config) extended 된 configuration 이다.                |
|                                   |                                                        | (derived config) base config 를 extend하는 configuration 이다. |
|                                   |                                                        | (resulting actual config) base config 로 derived config 을 merge 한 결과이다. |
|                                   | extends property value                                 | (a string) a config file 에 대한 a path 또는 a shareable config 의 name으로 a configuration 을 지정하는 문자열 |
|                                   |                                                        | (an array of strings) 각각의 추가적인 configuration 은 preceding configuration 을 extends 하는 문자열 배열 |
| rules 속성으로 extend             | enable additional rules                                |                                                              |
|                                   | rule 을 change                                         | base config: “eqeqeq”: [“error”, “allow-null”]               |
|                                   |                                                        | derived config: “eqeqeq”: “warn”                             |
|                                   |                                                        | resulting actual config: “eqeqeq”: [“warn”, “allow-null”]    |
|                                   | base config 로 부터 rules 에 대하여 options를 override | base config: “quotes”: [“error”, “single”, “avoid-escape”]   |
|                                   |                                                        | derived config: “quotes”: [“error”, “single”]                |
|                                   |                                                        | resulting actual config: “quotes”: [“error”, “single”]       |
| shareable config package 사용하기 | definition                                             | [a shareable configuration]()은 a configuration object 를 extend 하는 an npm package 이다. |
|                                   |                                                        | extends 속성 값은 패키지 이름의 eslint-config- 접두어를 생략할 수 있다. |
|                                   |                                                        | eslint \--init  명령으로  a configuraton을 만들 수 있기 때문에, 인기 있는 style guide를 extend 할 수 있다(예: eslint-config-standard) |

```json
{
  "extends": "stanadrd",
  "rules": {
    "comma-dangel": [
      - error,
      - always
    ],
    "no-empty": "warn"
  }
}
```

| extends | 구분                 | 설명                                                         |
| ------- | -------------------- | ------------------------------------------------------------ |
| extends | “eslint:recommended” | 일반적인 문제들을 레포트하는 core rules 의 a subset 을 enable 한다. |
|         |                      | 이러한  rules 들은 rules page 에서 체크마크 ✓로 분간된다.    |

```js
module.exports = {
    "extends": "eslint:recommended",
    "rules": {
        // enable additional rules
        "indent": ["error", 4],
        "linebreak-style": ["error", "unix"],
        "quotes": ["error", "double"],
        "semi": ["error", "always"],

        // override configuration set by extending "eslint:recommended"
        "no-empty": "warn",
        "no-cond-assign": ["error", "always"],

        // disable rules from base configurations
         "for-direction": "off",
    }
}
```

| extends | 구분            | 설명                                                         |
| ------- | --------------- | ------------------------------------------------------------ |
| extends | definition      | a plugin 은 ESLint로 여러 extensions 를 add 할 수 있는 an npm package 이다. |
|         |                 | a plugin 은 여러 함수들을 수행할 수 있고,  새로운 rules 를 add하고 shareable config 를 export 할 수 있다. |
|         |                 | plugin 속성 값은 패키지 이름의 eslint-plugin- 접두어를 생략할 수 있다. |
|         | extends 속성 값 | (plugin)                                                     |
|         |                 | (패키지 이름) 예: react (eslint-plugin-react 와 동일)        |
|         |                 | (config name) 예: recommended                                |

```json
{
    "plugins": [
        "react"
    ],
    "extends": [
        "eslint:recommended",
        "plugin:react/recommended"
    ],
    "rules": {
       "react/no-set-state": "off"
    }
}
```

## ESLint Language Options : env, globals, parserOptions

| Options | 구분     | 설명                                                         |
| ------- | -------- | ------------------------------------------------------------ |
| env     | 정의     | 작성한 스크립트가 실행하는 환경                              |
|         | browser  | browser 글로벌 변수들                                        |
|         | node     | NodeJS 글로벌 변수 및 NodeJS scoping                         |
|         | commonjs | CommonJS 글로벌 변수 및 CommonJS scoping(Browserify/WebPack 을 사용하는 browser-only  code 에 대하여 사용) |
|         | jest     | Jest 글로벌 변수                                             |
|         | mongo    | MongoDB 글로벌 변수                                          |
|         | 기타     | https://eslint.org/docs/user-guide/configuring/language-options#specifying-environments 참조 |

```json
# using configuration files
{
  "env": {
    "browser": true,
    "node": true
  }
}
```

```json
# using an environment from a plugin
{
  "plugins": ["example"],
  "env": {
    "example/custom": true
  }
}
```

| Options | 구분   | 설명                                                         |
| ------- | ------ | ------------------------------------------------------------ |
| globals | define | globals 라는 configuration property 를 설정하면, 글로벌 변수를 만들 수 있다. |
|         |        | “writable” 은 기존 변수를 overwrite 할 수 있고, “readonly” 는 기존 변수는 안된다. |

```json
{
  "globals": {
    "var1": "writable",
    "var2": "readonly"
  }
}
```

* globals 는 “off” 로 disable 할 수 있는데, 다음과 같이 작성하면, env 에서 ES2015 globals 는 사용가능하지만, Promise 는 사용가능하지 않게 된다.

```json
{
  "env": {
    "es6": true
  },
  "globals": {
    "Promise": "off"
  }
}
```

| Options       | 구분         | 설명                                                         |
| ------------- | ------------ | ------------------------------------------------------------ |
| parserOptions | define       | ESLint의 디폴트는 ECMAScript 5 syntax 이다.                  |
|               |              | parserOptions를 사용하면, JSX 뿐만 아니라 다른 ECMAScript 버전들도 enable 할 수 있다. |
|               |              | parserOptions를 사용하면, ESLint 가 a parsing error 를 결정할 수 있다. |
|               | ecmaVersioin | 사용하고자 하는 ECMAScript syntax 를 설정할 수 있다.         |
|               | sourceType   | script(디폴트) — ECMAScript modules 를 사용하는 경우에는 “module” 을 지정한다. |
|               | ecmaFeatures | (globalReurn) : global scope 에서 return statement 를 사용할 수 있다. |
|               |              | (impliedStrict) : ecmaVersion 이 5 이상이면, global strict mode 를 enable 할 수 있다. |
|               |              | (jsx) JSX 를 enable 할 수 있다.                              |

```json
{
  "parserOptions": {
    "ecmaVersion": "latest",
    "sourceType": "module",
    "ecmaFeatures": {
      "jsx": true
    }
  },
  "rules": {
    "semi": "error"
  }
}
```

## ESLint Rules 

| rules              | 구분           | 설명                                  |
| ------------------ | -------------- | ------------------------------------- |
| rule setting value | “off”          | rule 을 turn off 한다.                |
|                    | “warn” or “1”  | rule 을 warning으로 turn on 한다.     |
|                    |                | exit code 에 영향을 주지 않는다.      |
|                    | “error” or “2” | rule 을 error 로 turn on 한다.        |
|                    |                | 트리거가 되면, exit code 가 1이 된다. |

```json
{
  "rules": {
    "eqeqeq": "off",
    "curly": "error",
    "quotes": ["error", "double"] // 여기서 error는 error level이고, double 은 사용하고자 하는 옵션이다.
  }
}
```

| rule            | 구분   | 설명                                                         |
| --------------- | ------ | ------------------------------------------------------------ |
| Disabling Rules | define | 여러 파일들이 있을 때, overrides 키를 files 키와 함께 사용하면, rules 를 disable 할 수 있다. |

```json
{
  "rules": {....},
  "overrides": [
    {
      files: ["*-test.js", "*.spec.js"],
      rules: {
        "no-unused-expressions": "off"
      }
    }
  ]
}
```

## [ESLint Plugins: parser, processor, plugin](https://eslint.org/docs/user-guide/configuring/plugins)

| parser | 구분                      | 설명                                                         |
| ------ | ------------------------- | ------------------------------------------------------------ |
| parser | definition                | ESLint는Express를 디폴트 parser로 사용한다.                  |
|        | Esprima                   |                                                              |
|        | @babel/eslint-parser      | Babel parser 를 ESLint와 호환하게 만드는 wrapper 이다.       |
|        | @typescript-eslint/parser | ESLint 에서 사용할 수 있도록 TypeScript 를 ESTree-compatible form을 변환하는 parser 이다. |

```json
{
  "parser": "esprima",
  "rules": {
    "semi": "error"
  }
}
```

| processor | 구분       | 설명                                                         |
| --------- | ---------- | ------------------------------------------------------------ |
| processor | definition | plugins 는 processors 를 제공할 수도 있다.                   |
|           |            | processors 는 다른 종류의 파일들에서 JavaScript 코드를 extract 할 수 있다. |
|           |            | 따라서, ESLint 는 이 JavaScript 코드를 lint 할 수 있고, processors 는 특정 목적을 위해 preprocessing에서 JavaScript 코드를 변환할 수 있다. |

```json
# a-plugin 이라는 plugin이 제공하는 a-processor라는 processor를 enable 한다.
{
  "plugins": ["a-plugin"],
  "processor": "a-plugin/a-processor"
}
```



```json
# 특정 종류의 파일들에 대한 processors 를 지정하려면, overrides 키와 processor 키를 함께 사용한다. 
{
  "plugins": ["a-plugin"],
  "overrides": [
    {
      files: ["*.md"],
      "processor": "a-plugin/markdown"
    }
  ]
}
```

```json
# markdown 파일들에서 .js로 끝나는 named code blocks 에 대하여는 strict rule 을 disable 한다.
{
  "plugins": ["a-plugin"],
  "overrides": [
    {
      "files": ["*.md"],
      "processor": "a-plugin/markdown"
    },
    {
      "files": ["**/*.md/*.js"],
      "rules": {
        "strict": "off"
      }
    }
  ]
}
```

| Plugins | 구분       | 설명                                                        |
| ------- | ---------- | ----------------------------------------------------------- |
| plugins | definition | ESLint는 제3자 plugins 의 사용을 지원한다.                  |
|         |            | 제3자 plugin을 사용하기 위해서는 먼저 install 을 해야 한다. |

```json
{
  "plugins": [
    "plugin1",
    "eslint-plugin-plugin2" // eslint-plugin 접두어는 생략 가능
  ]
}
```

