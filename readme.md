## Cara membuat sitemap untuk web ReactJs dengan konten dinamis
![test](https://img.shields.io/npm/dt/react-router-sitemap-cashbac.svg?style=flat-square)
![test](https://img.shields.io/npm/v/react-router-sitemap-cashbac/latest.svg?style=flat-square)
![test](https://img.shields.io/badge/code_style-standard-brightgreen.svg?style=flat-square)

Posting ini menggambarkan bagaimana menggunakan [react-router-sitemap-cashbac](https://www.npmjs.com/package/react-router-sitemap-cashbac) untuk menghasilkan sitemap.xml file untuk aplikasi React Anda untuk membantu meningkatkan SEO pada website anda.

## Langkah 1 - Instal react-router-sitemap-cashbac
Langkah pertama adalah menginstal react-router-sitemap-cashbac menggunakan npm:
```
npm i --save react-router-sitemap-cashbac
```
## Langkah 2 - Buat file sitemap-routes.js
Langkah kedua adalah membuat file route duplikat hanya untuk tujuan sitemap. Berikut adalah contoh sitemap-routes.js
```
import React from 'react';
import { Route } from 'react-router';
 
export default (
    <Route>
	<Route path='/' />
	<Route path='/blog/:id' />
    </Route>
);
```
## Langkah 3 - Buat sitemap-generator.js
File ini akan melakukan pekerjaan berat untuk menghasilkan file sitemap.xml Anda. Berikut ini adalah contoh sederhana sitemap-generator.js
```
require("babel-register")({
  presets: ["es2015", "react"]
});
 
const router = require("./sitemap-routes").default;
const Sitemap = require("react-router-sitemap").default;

function generateSitemap() {
    return (
      new Sitemap(router)
          .build("https://www.example.com")
          .save("./public/sitemap.xml")
    );
}

generateSitemap();
```
Pastikan Anda mengubah path dan nama domain Anda.
## Langkah 4 - Jalankan file sitemap-generator.js
Instal dependensi dev berikut:
```
npm install --save-dev babel-cli
npm install --save-dev babel-preset-es2015
npm install --save-dev babel-preset-react
npm install --save-dev babel-register
```
Sekarang tambahkan skrip dibawah ini pada package.json anda
```...
"scripts": {
    ...
    "sitemap": "babel-node path/to/your/sitemap-generator.js"
  }
...
```
Sekarang jalankan npm run sitemap dan Voila! sitemap.xml telah terbentuk di directory public Anda. 

## Langkah 5 - Menambahkan jalur dinamis ke sitemap.xml Anda
Berikuta adalah sample script yang digunakan untuk membuat sitemap dinamis
```
require("babel-register")({
  presets: ["es2015", "react"]
});
 
const router = require("./sitemap-routes").default;
const Sitemap = require("react-router-sitemap").default;

const AWSAmplify = require("aws-amplify");
const Amplify = AWSAmplify.default;
const API = AWSAmplify.API;
const config = require("../config").default;

Amplify.configure({
  API: {
    endpoints: [
      {
        name: "posts",
        endpoint: config.apiGateway.URL,
        region: config.apiGateway.REGION
      },
    ]
  }
});

async function generateSitemap() {
  try {
    const posts = await API.get("posts", "/posts");
    let idMap = [];

    for(var i = 0; i < posts.length; i++) {
      idMap.push({ id: posts[i].postId });
    }

    const paramsConfig = {
      "/blog/:id": idMap
    };

    return (
      new Sitemap(router)
          .applyParams(paramsConfig)
          .build("https://www.example.com")
          .save("./public/sitemap.xml")
    );
  } catch(e) {
    console.log(e);
  } 
}

generateSitemap();
```

Itu saja untuk saat ini teman-teman :)

Terima Kasih.