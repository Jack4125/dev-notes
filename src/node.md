# Node

```js
// joi is form validation library
const Joi = require('joi');

/**
 * authentication and authorization
 * unit and integration testing
 */

const courses = [
  { id: 1, name: 'course 1' },
  { id: 2, name: 'course 2' },
  { id: 3, name: 'course 3' },
];

app.get('/api/courses/:id', (req, res) => {
  const course = courses.find((c) => c.id == req.params.id);

  if (!course) return res.status(404).send('Course with given id not found');
  res.send(course);
});

// ERROR: cannot post - did not have '/' at the beginning of the request '/api/courses'
app.post('/api/courses', (req, res) => {
  const schema = {
    name: Joi.string().min(3).required(),
  };

  const result = Joi.validate(req.body, schema);
  if (result.error) {
    res.status(400).send(res.error);
    return;
  }

  const course = { id: courses.length + 1, name: req.body.name };
  courses.push(course);
  res.send(course);
});

app.put('/api/courses/:id', (req, res) => {
  const course = courses.find((c) => c.id === parseInt(req.params.id));

  const schema = {
    name: Joi.string().min(3).required(),
  };

  const result = Joi.validate(req.body, schema);
  if (result.error) {
    res.status(400).send(res.error);
    return;
  }

  course.name = req.body.name;
  res.send(course);
});

app.get('/api/posts/:year/:month', (req, res) => {
  res.send(req.params);
});

app.get('/api/posts/:year/:month', (req, res) => {
  res.send(req.query);
  // http://localhost:3000/api/posts/2010/4?sortBy=name
});

// set environment variable with console command: set PORT=5000
const port = process.env.PORT || 3000;
app.listen(port, () => console.log(`server started on port ${port}`));
```
