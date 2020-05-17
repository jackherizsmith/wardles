# Sally Wardle, freelance journalist

![hero](https://imgur.com/F1Xs7bp)

This is the repository for [Sally Wardle's portfolio site](https://wardles.netlify.app) as a freelance journalist. It is deployed using [Netlify](https://www.netlify.com/) and [she has edit access](https://wardles.netlify.app/admin) using [Netlify CMS](https://www.netlifycms.org/). The site is compiled using the static site generator [Eleventy](https://www.11ty.dev/).

If you would like to fork the repo please feel free, you can run "npm start" so that Eleventy watches your files, it will generate `_site/` locally so you can visit *localhost:8080* and see wha tt he site will look like when deployed. Obviously the form won't submit anything since it is managed in production by Netlify Forms.

If it's of interest, I precompiled the CSS in Sass, which is not available in the repo. This could be an oversight. There is minimal Javascript, specifically to handle form validation (although again, form submission is handled by Netlify's native Form functionality and sent straight to Sally). There needs to be a bit more, which is detailed in the open issues.

You are free to peruse the issues and project board, and feel free to suggest any fixes or enhancements - I'm open to code reviews, just please tag them accordingly.

## How the site is generated by Eleventy

If you aren't familiar with Eleventy, I highly recommend it - [this video](https://www.youtube.com/watch?v=883iX2E57kc) was a great starting point for combining with Netlify's powerful deployment / hosting tools. I also ended up relying on [this guide](https://rphunt.github.io/eleventy-walkthrough/) a lot, a really helpful directory of information. Their template language of choice seems to be Nunjucks, which I've grown to love and has a single page of documentation [here](https://mozilla.github.io/nunjucks/templating.html).

*layouts* found in `src/_includes/`
- **base.njk** has the hero header image (only shows on Home), sticky navbar and footer
  - **home.njk** has the about section, featured articles, testimonials and contact form
  - **portfolio.njk** has the side navigation and hosts the article itself
  - **404.njk** should have a functional 404 page (but doesn't right now)
  
*collections* referred to in `src/admin/config.yml`, which Netlify uses to inform the CMS
- **portfolio** although maybe this should be article, is the tag on every .md file created via Netlify CMS
- **blurbs** (secionts Sally can edit)
  - **about** is for the about section
  - **collab**, whilst poorly named, is for what I think will end up being the testimonials section

*templates*
Each of these articles are saved in `src/portfolio/articles/article-slug-ie-headline-title-randomnumber`

Sally can upload an article with the following details (this is managed by Netlify CMS):
* **Featured?** if yes, will display article on homepage as clickable thumbnail
* **Title** headline, used in thumbnail, navigation and in the article itself
* **Journal** where it was published
* **Date** date of publication
* **Headline** main image
* **Thumbnail** thumbnail image
* **Content** the content of the article, which can be formatted by Sally using the native editor tools and hopfully is automatically reformatted by my CSS (which uses selectors rather than BEM considering limitations on parsing markdown templates).

Where articles are listed on the site, this comes from portfolioReversed in `.eleventy.js` which retrieves articles (collections.portfolioReversed) in chronological order, most recent first. You will also see in this file that you have to addPassthroughCopy() to allow non-template files to be parsed by Eleventy - e.g. Javascript, media etc.

## How the CMS is informed
The other side of the site is Netlify CMS, and I had a lot of ~lost hours~ fun working out `config.yml`. I'm still not great but lots of trial and error, a steep learning curve and two weeks later, I get it enough to do what I need it to do.

The main challenge was that I wanted to get all associated files in to the same folder as each other, but not with any other uploads related to other articles, and also for that folder to be uniquely named - e.g. with the headline slug. With Netlify CMS you tell it where to find extra files in the source code (`media_folder`) and where to find those files from the website (`public_folder`). Then you can reset that within collections which I did, so the *portfolio* collection has its own `folder`, a `path` within that holds each article, and then `media_folder` and `public_folder` are reset locally so that the new path is kept for uploads.

This meant that the website was calling for content from the global public folder, and so the workaround I ended up using was doubling up on retrieving data from each collection - its URL *and* its media since the latter didn't have a global path i.e. `{{article.url}}{{article.data.thumb}}`.

## Initial concept on Miro

![WardleS](https://imgur.com/EwoZ5Pv)

## Next steps
We'll see how the site develops as Sally adds more articles. I'm sure there are still bits to do, however I'm happy with its current state and so will probably get on with other projects for the immediate future.
