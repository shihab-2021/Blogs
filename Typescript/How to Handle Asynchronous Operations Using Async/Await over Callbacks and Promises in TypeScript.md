## How to Handle Asynchronous Operations Using Async/Await over Callbacks and Promises in TypeScript

In TypeScript, we have multiple ways to handle asynchronous operations: callbacks, promises, and async/await. Asynchronous programming allows us to manage operations that could take time, like fetching data from an API, without blocking the execution of other code.

### Callbacks

> A callback is a function passed as an argument to another function and is executed after a task completes. While callbacks work for simple tasks, they can quickly become unreadable when operations need to be chained.

```tsx
type Todo = {
  id: number;
  userId: number;
  title: string;
  completed: boolean;
};
const createPromise = (
  id: number,
  callback: (error: Error | null, task: Todo | null) => void
) => {
  fetch(`https://jsonplaceholder.typicode.com/todos/${id}`)
    .then((response) => {
      if (response.ok) {
        return response.json();
      } else {
        throw new Error("failed to load data");
      }
    })
    .then((data) => {
      callback(null, data);
    })
    .catch((error) => {
      callback(error, null);
    });
};

createPromise(1, (error, task) => {
  if (error) {
    console.error(error);
  } else {
    console.log(task);
  }
});
```

### Promises

> Promises provide a cleaner approach than callbacks by allowing us to handle asynchronous operations in a more linear way using .then() and .catch() methods. They’re easier to chain but can still get messy with complex operations.

```tsx
const createPromise = (id: number): Promise<object> => {
  return new Promise<object>((resolve, reject) => {
    const data: object = fetch(
      `https://jsonplaceholder.typicode.com/todos/${id}`
    ).then((response) => response.json());
    if (data) {
      resolve(data);
    } else {
      reject("failed to load data");
    }
  });
};

createPromise(1)
  .then((data) => console.log(data))
  .catch((error) => console.error(error));
```

### Async/Await

> Async/await provide a more readable and manageable way of handling asynchronous code compared to promises and callbacks. It lets us write asynchronous code as if it were synchronous, using the async keyword to mark functions as asynchronous and await to pause code execution until the promise resolves. This approach provides improved readability and is easier to debug.

```tsx
type Todo = {
  id: number;
  userId: number;
  title: string;
  completed: boolean;
};
const getTodo = async (): Promise<Todo> => {
  const response = await fetch("https://jsonplaceholder.typicode.com/todos/1");
  const data = await response.json();
  return data;
};

console.log(getTodo());
```

### Why Use Async/Await?

- Improved readability: Async/await is more readable and easier to follow, especially for complex code with multiple asynchronous calls.
- Error handling: It allows for simpler error handling with try/catch blocks, instead of multiple .catch() methods.
