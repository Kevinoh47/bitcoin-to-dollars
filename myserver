'use strict';

const fetch = require('node-fetch');
const crypto = require('crypto');
const qs = require('qs');
require('dotenv').config();

const express = require('express');

const app = express();
const PORT = process.env.PORT || 3030;

const apiKey = process.env.APIKEY;
const apiSecret = process.env.APISECRET;

app.set('view engine', 'ejs');

function makeRequest(verb, endpoint, data = {}) {
  const apiRoot = '/api/v1/';

  const expires = new Date().getTime() + (60 * 1000); // 1 min in the future

  let query = '', postBody = '';
  if (verb === 'GET')
    query = '?' + qs.stringify(data);
  else
    // Pre-compute the reqBody so we can be sure that we're using *exactly* the same body in the request
    // and in the signature. If you don't do this, you might get differently-sorted keys and blow the signature.
    postBody = JSON.stringify(data);

  const signature = crypto.createHmac('sha256', apiSecret)
    .update(verb + apiRoot + endpoint + query + expires + postBody).digest('hex');

  const headers = {
    'content-type': 'application/json',
    'accept': 'application/json',
    // This example uses the 'expires' scheme. You can also use the 'nonce' scheme. See
    // https://www.bitmex.com/app/apiKeysUsage for more details.
    'api-expires': expires,
    'api-key': apiKey,
    'api-signature': signature,
  };

  const requestOptions = {
    method: verb,
    headers,
  };
  if (verb !== 'GET') requestOptions.body = postBody; // GET/HEAD requests can't have body

  const url = 'https://www.bitmex.com' + apiRoot + endpoint + query;

  return fetch(url, requestOptions).then(response => response.json()).then(
    response => {
      if ('error' in response) throw new Error(response.error.message);
      return response;
    },
    error => console.error('Network error', error),
  );
}

async function main() {
  try {
    var results = await makeRequest('GET', `/instrument/compositeIndex?symbol=.XBT&filter=%7B%22timestamp.time%22%3A%2210%3A55%3A00%22%2C%22reference%22%3A%22BSTP%22%7D&count=100&reverse=true&?_format=JSON`
    );
    console.log('results:', results);
    return results;
  } catch (e) {
    console.error(e);
  }
}

// Routes
app.get('/', (request, response) => {
  main().then(result => { response.render('list',{items: result});});
});

app.listen(PORT, () => console.log(`Listening on ${PORT}`));