# Ohys-API

[![JavaScript Style Guide](https://img.shields.io/badge/code_style-standard-brightgreen.svg)](https://standardjs.com)

Ohys-API is a project to redistribute the anime metadata of Ohys-Raws.

## Table of Contents

- [Scripts](#scripts)
- [Development](#development)
  - [Environment](#environment)
  - [API](#api) (as module)

----

# Scripts

## `yarn start`

Start the application with logging only important messages.

## `yarn debug`

Start the application with logging all messages even from deps.

# Development

## Environment

Current application has been developed with Node.JS v14.5.0 and Yarnpkg v1.22.4.

# API

Also, you can install this project as your application's dependency and use functions.

```bash
# Install;
yarn add Seia-Soto/ohys-api
npm i git+https://github.com/Seia-Soto/ohys-api.git
```

```js
const { ohys } = require('Seia-Soto/ohys-api')
```

## ohys

### getFeed

Query Ohys JSON API.

```js
const { getFeed } = ohys

getFeed({
  page: 0,
  search: '',
  prettify: true // NOTE: This will patch the download url to full url.
})
  .then(data => {
    console.log(data)

/*
[
  // raw
  {
    t,
    a
  },
  // prettified
  {
    name: '...',
    link: 'https://eu.ohys.net/t/...'
  }
]
*/
  })
```

### getSchedule

Query the schedule file in modern format with fast search logic of title.

> **Warning**
> This method will only parse the schedule format in 2020 or later.

> **Warning**
> The schedule is not always able to parse if something change.

> **Warning**
> Using `getSchedulePattern` method is recommended.

```js
const { getSchedule } = ohys

getSchedule({
  year: 2020,
  quarter: 1,
  repo: 'ohyongslck/annie',
  branch: 'master'
})
  .then(data => {
    console.log(data)

/*
[
  {
    year: 2020,
    quarter: 1,
    day: 0, // NOTE: '7' is special.
    name: '...',
    comment: '...',
    original: '...'
  }
]
*/
  })
```

### getScheduleCompatible

Query the schedule file with heuristic search logic of title which can even parse 2019 or earlier, but this logic is slower than `getSchedule`.

> **Warning**
> The schedule is not always able to parse if something change.

> **Warning**
> Using `getSchedulePattern` method is recommended.

```js
const { getScheduleCompatible } = ohys
```

- The usage is same as `getSchedule`.

### getSchedulePattern

Query the schedule file with regular expression and heuristic search logic of title which can parse almost everything. (largest coverage)

> **Warning**
> If this logic doesn't work, use `getScheduleCompatible` instead.

```js
const { getSchedulePattern } = ohys

getSchedulePattern({
  year: 2020,
  quarter: 1,
  repo: 'ohyongslck/annie',
  branch: 'master'
})
  .then(data => {
    console.log(data)

/*
[
  {
    year: 2020,
    quarter: 1,
    day: 0, // NOTE: '7' is special.
    date: '00/00',
    time: '00:00',
    name: {
      promised: '...', // NOTE: (optional) You need to check this(`name.promised`) value to check if the value of title is present.
      English: '...', // NOTE: (optional)
      Korean: '...', // NOTE: (optional)
      Japanese: '...' // NOTE: (optional)
    },
    comment: '...',
    original: '...'
  }
]
*/
  })
```
