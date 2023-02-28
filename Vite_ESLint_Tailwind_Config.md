# Vite

Just type and follow steps: 
```
npm create vite@latest
```
<br>

# ESLint

Just type and follow steps: 
```
npm init @eslint/config
```
## Without TS:

```json
{
    "env": {
        "browser": true,
        "commonjs": true, //gets module exports
        "es2021": true,
        "node": true, 
        "jest": true //for jest library
    },
    "extends": "eslint:recommended",
    "overrides": [
    ],
    "parserOptions": {
        "ecmaVersion": "latest"
    },
    "rules":{
        "indent": [
            "error",
            2
        ],
        "linebreak-style": [
            "error",
            "unix"
        ],
        "quotes": [
            "error",
            "single"
        ],
        "eqeqeq": "error",
        "no-trailing-spaces": "error",
        "object-curly-spacing": [
            "error", "always"
        ],
        "arrow-spacing": [
            "error", { "before": true, "after": true }
        ],
        "no-console": 0,
        "max-len": ["error", { "code": 80 }],
        "multiline-ternary": ["error", "always"],
        "no-multi-spaces": "error"
    }
}
```

<br>

# Tailwind CSS

Just type and follow steps: 
```
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

Some config:

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./index.html", "./src/**/*.{js,jsx}"],
  mode: "jit",
  theme: {
    extend: {
      colors: {
        primary: "#00040f",
        secondary: "#00f6ff",
        dimWhite: "rgba(255, 255, 255, 0.7)",
        dimBlue: "rgba(9, 151, 124, 0.1)",
      },
      fontFamily: {
        poppins: ["Poppins", "sans-serif"],
      },
    },
    screens: {
      xs: "480px",
      ss: "620px",
      sm: "768px",
      md: "1060px",
      lg: "1200px",
      xl: "1700px",
    },
  },
  plugins: [],
};
```

Add to the `index.css` :

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

:root {

}
```