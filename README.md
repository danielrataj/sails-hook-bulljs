# sails-hook-bulljs

Sails.js hook to use [bull] package for handling distributed jobs and messages.

Based on [sails-hook-bull] package (thanks).

## Installation

Install it via npm:

```shell
npm install sails-hook-bulljs --save
```

or

```shell
yarn add sails-hook-bulljs
```

## Configuration

You can set some parameters in `config/bulljs.js` to override defaults options for all queues.

```javascript
module.exports.bulljs = {
    redis: {
        port: 6379,
        host: '127.0.0.1',
        db: 0,
        password: ''
    }
}
```

See [bull.queue] for more information.

## Examples

Bull queue definition in `api/queues/my-queue.js`

```javascript
module.exports = {
  queueName: 'my-queue',
  // if redis already defined as an object, url will be ignored
  url: 'redis://127.0.0.1:6379',
  opts: {
    limiter: {
        max: 1000,
        duration: 5000
    }
  },
  process: async job => {
    // process queue
  },
  onCompleted: async job => {
    // do something when completed
  },
  onRemoved: async job => {
    // do something when job removed
  },
  onError: async err => {
    // do something on error like Redis connection failure etc.
  },
  onFailed: async (job, err) => {
    // do something when process fails
  },
};
```

You can define queues with options:

```javascript
module.exports = {
  queueName: 'my-queue',
  process: {
    name: 'my-super-queue',
    concurrency: 1,
    processor: (job, done) => {
      // do something...
      done();
    }
  }
};
```

Adding jobs to a queue:

```javascript
// api/controllers/someController.js
module.exports = {
  someAction: function(req, res) {
    sails.hooks.bulljs.myQueue.add({
        param: 'value'
    })
  }
};
```

## License

MIT

[bull]: https://optimalbits.github.io/bull/

[bull.queue]: https://github.com/OptimalBits/bull/blob/master/REFERENCE.md#queue

[sails-hook-bull]: https://www.npmjs.com/package/sails-hook-bull
