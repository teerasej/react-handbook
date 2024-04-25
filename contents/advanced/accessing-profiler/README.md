
# Accessing Profiler in React DevTools

The React DevTools Profiler is a tool that helps you identify performance issues in your React application. It allows you to visualize the components that are rendering and how long it takes to render them. In this guide, we will show you how to access the Profiler in React DevTools.

## 1. Install React DevTools

If you haven't already installed React DevTools, you can do so by following the instructions in the [official documentation](https://react.dev/learn/react-developer-tools).

## 2. Create a react project

Create a new React project using the following command:

```bash
npx create-react-app profiler-example
```

then run the project:

```bash
cd profiler-example
npm start
```

## 3. Open React DevTools

<img width="742" alt="2024-04-25_22-16-08 (1)" src="https://github.com/teerasej/react-handbook/assets/85179/cb5eb26e-b975-47d2-8547-b49b7bbd14c3">

1. Open your browser's developer tools by right-clicking on the page and selecting "Inspect" or pressing `F12`. 
2. Go to the "Profiler" tab
3. click 'reload and start profiling' button to start profiling your application.
4. click 'stop profiling' button to stop profiling your application.
5. review the performance of your application.
