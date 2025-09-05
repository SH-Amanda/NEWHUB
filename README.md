# NEWHUB
gitbub上的第一个存储
{
  "name": "NEWHUB",
  "version": "1.0.0",
  "main": "dist/app.js",
  "scripts": {
    "start": "tsc && node dist/app.js",
    "dev": "ts-node-dev src/app.ts",
    "test": "jest"
  },
  "dependencies": {
    "express": "^4.18.2"
  },
  "devDependencies": {
    "@types/express": "^4.17.17",
    "@types/jest": "^29.5.2",
    "jest": "^29.6.1",
    "ts-jest": "^29.1.1",
    "ts-node-dev": "^2.0.0",
    "typescript": "^5.1.3"
  }
}
{
  "compilerOptions": {
    "target": "ES6",
    "module": "CommonJS",
    "outDir": "dist",
    "rootDir": "src",
    "strict": true,
    "esModuleInterop": true
  }
}
export interface Todo {
  id: number;
  title: string;
  completed: boolean;
}

export let todos: Todo[] = [];
import express from 'express';
import { getTodos, addTodo, completeTodo, deleteTodo } from './controllers';

const router = express.Router();

router.get('/todos', (req, res) => {
  res.json(getTodos());
});

router.post('/todos', (req, res) => {
  const { title } = req.body;
  const todo = addTodo(title);
  res.status(201).json(todo);
});

router.put('/todos/:id/complete', (req, res) => {
  const id = Number(req.params.id);
  const todo = completeTodo(id);
  if (todo) res.json(todo);
  else res.status(404).send('Todo not found');
});

router.delete('/todos/:id', (req, res) => {
  const id = Number(req.params.id);
  if (deleteTodo(id)) res.status(204).send();
  else res.status(404).send('Todo not found');
});

export default router;
