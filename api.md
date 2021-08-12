# Sitemap

Generate a sitemap using the [React Router](https://www.npmjs.com/package/react-router) configuration.

**Examples**

```javascript
import Sitemap from 'react-router-sitemap-cashbac';

const sitemap = (
  new Sitemap(<Route path='/home'>)
    .build('http://my-site.ru')
    .save("./sitemap.xml");
);
```
