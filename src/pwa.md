# Progressive Web App

## Sources

https://www.w3schools.com/html/html5_webstorage.asp
https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API

https://www.youtube.com/watch?v=g4U5WRzHitM

## Service Worker

### sw.js

```js
/*
https://developers.google.com/web/fundamentals/primers/service-workers/#the_defaults_of_fetch

One subtlety with the register() method is the location of the 
service worker file. You'll notice in this case that the service 
worker file is at the root of the domain. This means that the 
service worker's scope will be the entire origin. In other words, 
this service worker will receive fetch events for everything on this 
domain. If we register the service worker file at /example/sw.js, 
then the service worker would only see fetch events for pages whose 
URL starts with /example/ (i.e. /example/page1/, /example/page2/).
*/

// https://www.youtube.com/watch?v=ksXwaWHCW6k
const cacheName = 'v1';
// caching essential, non-homepage files on first load
// const cacheAssets = ["resume.html"];
const cacheAssets = [];

// Step 2 of SW lifecycle - install
// Here, we can cache static assets
// If caching fails, SW will not move on to step 3 "activate"
self.addEventListener('install', (e) => {
  console.log('Service Worker installed');
  // wait until promise is finished before closing the service worker
  e.waitUntil(
    // e.waitUntil() method takes a promise and uses it to know how
    // long installation takes, and whether it succeeded or not.
    caches
      .open(cacheName)
      .then((cache) => {
        console.log('Opened Cache');
        cache.addAll(cacheAssets);
      })
      .then(() => self.skipWaiting())
  );
});

// Step 3???????????? of SW Lifecycle - activate
// Here, we manage old caches
// after activation, the service worker will be in 2 states:
// either terminated, or (Step 4) handle fetch and message events
// when a network request is made
self.addEventListener('activate', (e) => {
  console.log('Service Worker activated');
  // remove unwanted caches
  e.waitUntil(
    caches.keys().then((cacheNames) => {
      return Promise.all(
        cacheNames.map((cache) => {
          if (cache !== cacheName) {
            console.log('Service Worker clearing old cache');
            return caches.delete(cache);
          }
        })
      );
    })
  );
});

// fetch event happens when either the user navigates to a different
// page, or when the page refreshes
self.addEventListener('fetch', (e) => {
  console.log('Service Worker fetching');
  e.respondWith(
    fetch(e.request)
      .then((res) => {
        const resClone = res.clone();
        caches.open(cacheName).then((cache) => {
          cache.put(e.request, resClone);
        });
        return res;
      })
      .catch(() => caches.match(e.request).then((res) => res))
  );
});
```

### script.js

```js
// Service Worker
// https://www.youtube.com/watch?v=ksXwaWHCW6k

// this is called the "control page"

// You can call register() every time a page loads without concern;
// the browser will figure out if the service worker is already
// registered or not and handle it accordingly.
if (navigator.serviceWorker) {
  window.addEventListener('load', () => {
    navigator.serviceWorker
      .register('./sw.js') // set 1 of SW lifecycle - register to install
      .then((reg) => console.log('Service Worker registered'))
      .catch((err) => console.log(`Error: ${err}`));
  });
}

// Push Notification
Notification.requestPermission((status) => {
  console.log('Notification permission status:', status);
});

function displayNotification() {
  if (Notification.permission == 'granted') {
    navigator.serviceWorker.getRegistration().then((reg) => {
      var options = {
        body: 'Fill in the form and a resume will be generated for you.',
        icon: 'images/favicon-32x32.png',
        vibrate: [100, 50, 100],
        data: {
          dateOfArrival: Date.now(),
          primaryKey: 1,
        },
        actions: [
          {
            action: 'explore',
            title: 'Explore',
            icon: 'images/favicon-32x32.png',
          },
          {
            action: 'close',
            title: 'Close',
            icon: 'images/favicon-32x32.png',
          },
        ],
      };
      reg.showNotification('Welcome to Resume Generator!', options);
    });
  }
}

displayNotification();

// IndexedDB
// https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API/Using_IndexedDB
// http://www.tutorialspoint.com/html5/html5_indexeddb.htm

// Put everything into a function called by submit.
// That way, the indexedDB isn't created as soon as page loads, but when user actually submits.

// Uses IndexedDBShim as a final fallback
var indexedDB =
  window.indexedDB ||
  window.mozIndexedDB ||
  window.webkitIndexedDB ||
  window.msIndexedDB ||
  window.shimIndexedDB;

var request = indexedDB.open('resumeDB', 1); // change version only when schema is changed
// RETURNS a response to be used for "onupgradeneeded, onerror, or onsuccess"
// if database/"resumeDB" doesn't exist yet, create it, and trigger onupgradeneeded
// if database exists but version number if higher, trigger onupgradeneeded for new version/schema

let db;

request.onsuccess = (e) => {
  db = request.result; // request.result is an "instance of the database", so we save it for future use
};

request.onerror = (e) => {
  console.log('Error: ' + e.target.errorCode);
};

// onupgradeneeded is used to create/update an objectStore
request.onupgradeneeded = (e) => {
  request.result.createObjectStore('resumeObjectStore', {
    keyPath: 'id', // 1. define keyPath to be 'id' entry
  });
};

document.getElementById('submit').addEventListener('click', (e) => {
  e.preventDefault();
  idbSave();
  idbGenerate();
  useCursor();
  useGetAll();
});

function idbSave() {
  // open transaction and access object store
  var store = db
    .transaction(['resumeObjectStore'], 'readwrite') // ? transaction and objectStore same name?
    .objectStore('resumeObjectStore');

  // add data to store
  store.put({
    id: '1', // 2. the name 'id' is used during definition, and value "1" will be used to locate the object below
    name: document.getElementById('name').value,
    address: document.getElementById('address').value,
    phone: document.getElementById('phone').value,
    email: document.getElementById('email').value,
    skillStrong: document.getElementById('skill-strong').value,
    skillWeak: document.getElementById('skill-weak').value,
    project1: document.getElementById('project-1').value,
    project1Desc: document.getElementById('project-1-desc').value,
    project2: document.getElementById('project-2').value,
    project2Desc: document.getElementById('project-2-desc').value,
    project3: document.getElementById('project-3').value,
    project3Desc: document.getElementById('project-3-desc').value,
    work1: document.getElementById('work-1').value,
    work1Desc: document.getElementById('work-1-desc').value,
    work2: document.getElementById('work-2').value,
    work2Desc: document.getElementById('work-2-desc').value,
    work3: document.getElementById('work-3').value,
    work3Desc: document.getElementById('work-3-desc').value,
    education: document.getElementById('education').value,
  });
}

function idbGenerate() {
  db.transaction('resumeObjectStore').objectStore('resumeObjectStore').get('1'); // 3. using the 'value' part of the primary key to located item

  request.onerror = () => {
    alert('Unable to retrieve data from database');
  };

  request.onsuccess = () => {
    if (request.result) {
      console.log(
        'Name: ' +
          request.result.name +
          ', Address: ' +
          request.result.address +
          ', Email: ' +
          request.result.email
      );
    } else {
      console.log('Cannot access your form');
    }
  };
}

// Cursor is used when looping through all values in object store, without knowing the key
function useCursor() {
  db
    .transaction('resumeObjectStore')
    .objectStore('resumeObjectStore')
    .openCursor().onsuccess = (e) => {
    var cursor = e.target.result;
    if (cursor) {
      console.log(
        'Cursor generated: Name is ' +
          cursor.value.name +
          ' Address is ' +
          cursor.value.address
      );
      cursor.continue();
    } else {
      console.log('No more entries');
    }
  };
}

function useGetAll() {
  db
    .transaction('resumeObjectStore')
    .objectStore('resumeObjectStore')
    .getAll().onsuccess = (e) => {
    console.log('getAll generated: ' + e.target.result);
  };
}
```

## Storage

To see all values, either loop through storage with Object.key(), or go
to Developer Tools > Application tab > Storage

```js
// local storage ==========================================================
document.getElementById('local-add').addEventListener('click', () => {
  let name = document.getElementById('local-name').value;
  let value = document.getElementById('local-value').value;
  localStorage.setItem(name, value);
  output('local', 'add', name);
});

document.getElementById('local-delete').addEventListener('click', () => {
  let name = document.getElementById('local-name').value;
  localStorage.removeItem(name);
  output('local', 'delete', name);
});

// session storage ==========================================================
document.getElementById('session-add').addEventListener('click', () => {
  let name = document.getElementById('session-name').value;
  let value = document.getElementById('session-value').value;
  sessionStorage.setItem(name, value);
  output('session', 'add', name);
});

document.getElementById('session-delete').addEventListener('click', () => {
  let name = document.getElementById('session-name').value;
  sessionStorage.removeItem(name);
  output('session', 'delete', name);
});

// generate output
function output(type, action, name) {
  let html = '<div>';
  if (type === 'local') {
    if (action === 'add') {
      html += `<p>Name: ${name}</p><p>Value: ${localStorage.getItem(name)}</p>`;
    } else {
      html += `<p>"${name}" is deleted from Local Storage. It's value now is: ${localStorage.getItem(
        name
      )}</p>`;
    }
  } else {
    if (action === 'add') {
      html += `<p>Name: ${name}</p><p>Value: ${sessionStorage.getItem(
        name
      )}</p>`;
    } else {
      html += `<p>"${name}" is deleted from session Storage. It's value now is: ${sessionStorage.getItem(
        name
      )}</p>`;
    }
  }
  html += '</div>';
  document.getElementById('output').innerHTML = html;
}

// IndexedDB ==========================================================
// Allows storage of objects accessed through keys.
// Asynchronous: establish connection with indexedDB, wait for it to respond with event, then continue.
/* Two Parts
  Part I - initial setup:
    1. open a database
    2. create an object store (table) in database - define structure here, specify key
    3. on success of step 2, conduct INITIAL transaction
    4. close transaction
  Part II - Each transaction
    1. open object store
    3. on success of step 1, conduct transaction
    4. close transaction
  */

// https://developers.google.com/web/ilt/pwa/working-with-indexeddb
// https://developer.mozilla.org/en-US/docs/Web/API/IDBTransaction/oncomplete
// http://www.tutorialspoint.com/html5/html5_indexeddb.htm
// https://flaviocopes.com/indexeddb/

// ERROR: if put () behind idbAdd, function is immediately called.
document.getElementById('idb-add').addEventListener('click', idbAdd);
document.getElementById('idb-get').addEventListener('click', idbGet);
document.getElementById('idb-delete').addEventListener('click', idbDelete);

// Uses IndexedDBShim as a final fallback
// var indexedDB =
//   window.indexedDB ||
//   window.mozIndexedDB ||
//   window.webkitIndexedDB ||
//   window.msIndexedDB ||
//   window.shimIndexedDB;

/**
 * Part I - Setup ============================================
 */
// I-1. Open (or create) the database
const request = window.indexedDB.open('helloDatabase', 1);

request.onupgradeneeded = (event) => {
  console.log('entered upgrade stuff');
  const db = event.target.result;
  const store = db.createObjectStore('myObjectStore', { keyPath: 'id' }); // or {autoIncrement: true}

  // 'index' is not always needed. Only need 'index' on column names that you want to searching by
  var index = store.createIndex('nameIndex', ['name.last', 'name.first']); // opt. {unique: true}
};

request.onerror = (e) => {
  console.log('Error: ' + e.target.errorCode);
};

// I-3.
request.onsuccess = function () {
  // Start a new transaction
  var db = request.result;
  var tx = db.transaction('myObjectStore', 'readwrite');
  console.log('IndexedDB initial setup is successful.');

  // I-4. Close the db when the transaction is done
  tx.oncomplete = () => {
    db.close();
  };
};

/**
 *  Part II - Transactions ====================================
 */
function idbAdd() {
  console.log('entered idbadd');
  const request = window.indexedDB.open('helloDatabase', 1);

  request.onsuccess = () => {
    const db = request.result;
    // II-1. identify store - can have more than one
    var tx = db.transaction('myObjectStore', 'readwrite');
    var store = tx.objectStore('myObjectStore');
    var index = store.index('nameIndex');

    // II-2. Action
    let id = document.getElementById('idb-id').value;
    let first = document.getElementById('idb-first').value;
    let last = document.getElementById('idb-last').value;
    let age = document.getElementById('idb-age').value;

    // Add data
    store.put({ id: id, name: { first: first, last: last }, age: age });

    // II-3. Close the db when the transaction is done
    tx.oncomplete = () => {
      db.close();
    };
  };
}

function idbGet() {
  console.log('entered idbget');
  const request = window.indexedDB.open('helloDatabase', 1);

  request.onsuccess = () => {
    const db = request.result;
    // identify store - can have more than one
    var tx = db.transaction('myObjectStore', 'readwrite');
    var store = tx.objectStore('myObjectStore');
    var index = store.index('nameIndex');

    let id = document.getElementById('idb-id').value;
    let first = document.getElementById('idb-first').value;
    let last = document.getElementById('idb-last').value;

    // Query the data
    var getWithId = store.get(id); // retrieve using store
    var getWithName = index.get([last, first]); // retrieve using index
    // asynchronous - returns result as 'getJohn' or 'getBob'
    // the result is the whole object in store.put()

    getWithId.onsuccess = function () {
      console.log('Gotten with ID ' + getWithId.result.name.first); // => "John"
    };

    getWithName.onsuccess = function () {
      console.log('Gotten with Name ' + getWithName.result.name.first); // => "Bob"
    };

    // II-3. Close the db when the transaction is done
    tx.oncomplete = () => {
      db.close();
    };
  };
}

function idbDelete() {
  console.log('entered idbdelete');
  const request = window.indexedDB.open('helloDatabase', 1);

  request.onsuccess = () => {
    const db = request.result;
    // identify store - can have more than one
    var tx = db.transaction('myObjectStore', 'readwrite');
    var store = tx.objectStore('myObjectStore');
    var index = store.index('nameIndex');

    let id = document.getElementById('idb-id').value;

    // delete the data
    store.delete(id); // retrieve using store

    // II-3. Close the db when the transaction is done
    tx.oncomplete = () => {
      db.close();
    };
  };
}
```

### Indexedb

```html
<button onclick="createInvoice()">Create</button>
<button onclick="readInvoice()">Read</button>
<button onclick="updateInvoice()">Update</button>
<button onclick="deleteInvoice()">Delete</button>
```

```js
const request = window.indexedDB.open('invoiceDatabase', 1);

request.onupgradeneeded = (event) => {
  const db = event.target.result;

  const invoiceStore = db.createObjectStore('invoices', {
    keyPath: 'invoiceId',
  });
  invoiceStore.createIndex('VendorIndex', 'vendor');

  const itemStore = db.createObjectStore('invoice-items', {
    keyPath: ['invoiceId', 'row'],
  });
  itemStore.createIndex('InvoiceIndex', 'invoiceId');

  const fileStore = db.createObjectStore('attachments', {
    autoIncrement: true,
  });
  fileStore.createIndex('InvoiceIndex', 'invoiceId');
};

// Request Error
request.onerror = (e) => {
  console.log('Error: ' + e.target.errorCode);
};

// Create ==================================================
function createInvoice() {
  const request = window.indexedDB.open('invoiceDatabase', 1);
  request.onsuccess = () => {
    const db = request.result;
    const transaction = db.transaction(
      ['invoices', 'invoice-items'],
      'readwrite'
    );
    const invStore = transaction.objectStore('invoices');
    const itemStore = transaction.objectStore('invoice-items');

    // add
    invStore.add({ invoiceId: '123', vendor: 'Whirlpool', paid: false });

    itemStore.add({
      invoiceId: '123',
      row: '1',
      item: 'Dish washer',
      cost: 1400,
    });
    itemStore.add({
      invoiceId: '123',
      row: '2',
      item: 'Labor',
      cost: 500,
    });

    // Clean up: close connection
    transaction.oncomplete = () => {
      db.close();
    };
  };
}

// Read ==================================================
function readInvoice() {
  const request = window.indexedDB.open('invoiceDatabase', 1);
  request.onsuccess = () => {
    // 1. start transaction on databases 'invoices, invoice-items'
    const db = request.result;
    const transaction = db.transaction(
      ['invoices', 'invoice-items'],
      'readwrite'
    );
    // 2. pull out the object stores from returned transaction object
    const invStore = transaction.objectStore('invoices');
    const itemStore = transaction.objectStore('invoice-items');

    // 3. get the needed entry from the store
    const invStoreRequest = invStore.get('123');
    invStoreRequest.onsuccess = () => {
      console.log('invoices' + invStoreRequest.result);
    };

    const itemStoreRequest = itemStore.get(['123', '1']);
    itemStoreRequest.onsuccess = () => {
      console.log('invoice-items' + itemStoreRequest.result);
    };

    // 4. close transaction when done
    transaction.oncomplete = () => {
      db.close();
    };
  };
}

// Update ==================================================
function updateInvoice() {
  const request = window.indexedDB.open('invoiceDatabase', 1);
  request.onsuccess = () => {
    const db = request.result;
    const transaction = db.transaction(
      ['invoices', 'invoice-items'],
      'readwrite'
    );
    const itemStore = transaction.objectStore('invoice-items');

    itemStore.put({
      invoiceId: '123',
      row: '1',
      item: 'Dish washer',
      cost: 1300,
    });

    transaction.oncomplete = () => {
      db.close();
    };
  };
}

// Delete ==================================================
function deleteInvoice() {
  const request = window.indexedDB.open('invoiceDatabase', 1);
  request.onsuccess = () => {
    const db = request.result;
    const transaction = db.transaction(
      ['invoices', 'invoice-items'],
      'readwrite'
    );
    const itemStore = transaction.objectStore('invoice-items');

    // delete
    itemStore.delete(['123', '2']);

    transaction.oncomplete = () => {
      db.close();
    };
  };
}
```
