# Async Error Handler for Express

This utility function wraps async route handlers to automatically catch and forward errors to Express error handling middleware.

```typescript
import { NextFunction, Request, RequestHandler, Response } from 'express';

export function catchAsync(
  fn: (req: Request, res: Response, next: NextFunction) => Promise<void>,
): RequestHandler {
  return (req: Request, res: Response, next: NextFunction) => {
    Promise.resolve(fn(req, res, next)).catch(next);
  };
}

// Usage example:
// Without catchAsync (requires try-catch)
app.get('/users', async (req: Request, res: Response, next: NextFunction) => {
  try {
    const users = await User.find();
    res.json(users);
  } catch (error) {
    next(error);
  }
});

// With catchAsync (cleaner syntax)
app.get(
  '/users',
  catchAsync(async (req: Request, res: Response) => {
    const users = await User.find();
    res.json(users);
  }),
);
```
