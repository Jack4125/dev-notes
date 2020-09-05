---
title: "Deployment"
tags: "Netlify, Github, Heroku"
---

# Deployment

- Netlify

  - All-in-one (DNS, CDN, CI, cloud functions, etc.)
  - Easy deploy (with or without Git tool)

- GitHub

  - Static only, no server
  - Auto-deploy from `index.html`
  - [GitHub Pages custom url](https://medium.com/employbl/launch-a-website-with-a-custom-url-using-github-pages-and-google-domains-3dd8d90cc33b)
  - [GitHub Pages Google Domain](http://www.curtismlarson.com/blog/2015/04/12/github-pages-google-domains/)

- Heroku
  - Complicated
  - Server side
  - Language specific deployment requirements.

## Hosting

### CNAME

- Points FROM _where the file is located_ TO _the address inside the file_.

- (Github pages) Add a CNAME file in root directory. Inside, list the DNS name that was registered that you want the github page to redirect to

  - Name: `www`
  - Type: `CNAME`
  - Target: `host.temporaryname.com`

## DNS

[Free DNS](https://www.freenom.com/)

[Name servers and CNAME explained](https://www.reddit.com/r/github/comments/97vqbt/does_github_pages_not_have_nameservers/)

Domains that don't end with `.com` `.net` `.org` may be bad at SEO.

### A (address) Record

- Name: leave empty or `@`
- Type: `A` for "Address"
- Target: ALL [ip addresses of hosting server](https://stackoverflow.com/questions/9082499/custom-domain-for-github-project-pages) (Github: `185.199.108.153`, `185.199.109.153`, etc.)

### Nameserver

- User default. Don't change.

## Heroku

### Static

Heroku does _NOT_ automatically use `index.html` as entry point, but it does use `index.php` as entry point, so converting static sites to PHP is a get-around.

1. Go to Heroku -> app_name -> Settings -> Buildpacks -> `Add buildpack`
2. Add `heroku/php`
3. In application root, add a `index.php` file
4. Inside it, write:
   ```php
   <?php include_once("index.html"); ?>
   ```
5. Push to Heroku.

### Dynamic

1. Set up server
2. Include Heroku specific configurations in application.
3. Follow Heroku language-specific setup guideline.

## Errors

- [GitHub Pages Custom Domain Won't Update](https://stackoverflow.com/questions/9130422/how-long-do-browsers-cache-http-301s)
  - Solution: On GitHub settings page, F12 -> Network tab -> check `Disable cache` -> refresh page -> retry link
- Browser still use cache pages after source code update (git push):
  - Dev Console -> Network -> Disable Cache
  - Dev Console -> Application -> Clear Storage
