# nuisance

[![Current Version](https://img.shields.io/npm/v/nuisance.svg)](https://www.npmjs.org/package/nuisance)
[![Build Status via Travis CI](https://travis-ci.org/continuationlabs/nuisance.svg?branch=master)](https://travis-ci.org/continuationlabs/nuisance)
![Dependencies](http://img.shields.io/david/continuationlabs/nuisance.svg)

[![belly-button-style](https://cdn.rawgit.com/continuationlabs/belly-button/master/badge.svg)](https://github.com/continuationlabs/belly-button)

`nuisance` is a hapi plugin that allows multiple authentication strategies to be aggregated into a single strategy. hapi allows you specify multiple strategies on a route, however this approach only requires that a single strategy is successful. `nuisance`, on the other hand, requires that **all** of the strategies are successful.

If all of the authentication strategies are successful, `request.auth.credentials` will be populated with the credentials from each strategy. If any of the strategies fail, an Unauthorized error will be returned.

## Example

```javascript
// Given the following auth strategies in your server:
server.auth.strategy('fooAuth', 'scheme', options);
server.auth.strategy('barAuth', 'scheme', options);
server.auth.strategy('bazAuth', 'scheme', options);

// You can define an aggregate strategy based on the nuisance scheme:
server.auth.strategy('fooBarBaz', 'nuisance', {
  strategies: ['fooAuth', 'barAuth', 'bazAuth']
});

// Then use the aggregate strategy as needed:
server.route([
  {
    method: 'GET',
    path: '/foo/bar/baz',
    config: {
      auth: 'fooBarBaz',
      handler: function (request, reply) {
        reply('ok');
      }
    }
  }
]);
```

## Configuration Options

- `strategies` (array) - An array of strings, representing the authentication strategies to be tested.
