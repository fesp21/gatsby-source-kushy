# gatsby-source-kushy

Source plugin for pulling a shop or brand's data into Gatsby from the [Kushy API](http://kushy.net).

Add your shop or brand to the configuration file and Gatsby will import your profile data, menu, and all your reviews and photos into GraphQL. 

Great for creating a website or landing page for a dispensary or brand.

* Demo
* Example site source code

## Getting Started

1. `npm install --save gatsby-source-kushy`
1. Add the plugin to `gatsby-config.js`:

```json
    {
      resolve: 'gatsby-source-kushy',
      options: {
        // Access the shops or brands section?
        section: 'shops',
        // The slug of your shop/brand
        slug: 'chronic-pain-relief-center',
        // This helps the source plugin use the Kushy API to find your shop
        // e.g. http://api.kushy.net/v1/<section>/?filter[slug]=<slug>
        // See Kushy API docs for more info on querying shop data
      },
    },
```

3. `gatsby develop`
4. [Use the GraphiQL browser to query Kushy data](http://localhost:8000/___graphql?query=%7B%0A%20%20allKushyPost%7B%0A%20%20%20%20edges%20%7B%0A%09%09%09node%20%7B%0A%20%20%20%20%20%20%20%20id%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A)

## Querying with GraphQL

The plugin creates an endpoint called `KushyPost` and `allKushyPost`. You query `KushyPost` for individual posts and filter by an ID or slug. And you query `allKushyPost` for all imported posts.

All the parameters from the API are plugged into GraphQL, so if an API endpoint has a `avatar` property, you can query GraphQL for it.

### Query your shop/brand

```graphql
{
  allKushyPost{
    edges {
			node {
        id
        name
        featured_img
        avatar
        location {
          latitude
          longitude
          address
          state
          city
          postal_code
          country
        }
        rating
        featured
        verified
      }
    }
  }
}
```

### Query your reviews

```graphql
{
  allKushyReview {
    edges {
      node{
        id
        status
        post_id
        user_id
        rating
        useful
        dank
        funny
        review
        created_at {
					date
        }
        updated_at {
          date
        }
        includes {
					user {
            id
            name
            username
            avatar
          }
        }
      }
    }
  }
}
```

### Query your menu

```graphql
{
  allKushyMenu {
    edges {
      node{
        id
        product {
          id
          name
          slug
          categories
          avatar
          featured_img
        }
        pricing {
          pricing_type
          list_price
          sale_price
          half_gram
          one_gram
          two_grams
          eighth_ounce
          quarter_ounce
          half_ounce
          ounce
          quarter_pound
          half_pound
          pound
        }
        cannabinoids {
          thc
          cbd
          cbn
        }
      }
    }
  }
}
```

### Query your photos

```graphql
{
  allKushyPhoto {
    edges {
      node {
        id
        image
        caption
        user_id
        post_id
        featured
        created_at{
          date
          timezone
        }
        updated_at{
          date
          timezone
        }
      }
    }
  }
}
```

## Todo

* [✅] - Allow users to add a config for section and ID to load a specific shop/brand.
* [✅] - Add endpoint for shop/brand menu (`KushyMenu` / `allKushyMenu`)
* [] - Add endpoint for shop/brand photos (`KushyPhotos` / `allKushyPhotos`)
* [✅] - Add endpoint for shop/brand reviews (`KushyReviews` / `allKushyReviews`)

### Pending

* [] - Create async function to load all pages from API, currently only loads first (see: [getPages()](https://github.com/gatsbyjs/gatsby/blob/master/packages/gatsby-source-wordpress/src/fetch.js#L334-L348))
* [] - Create helper function to download all loaded media into cache (see: [downloadMediaFiles()](https://github.com/gatsbyjs/gatsby/blob/master/packages/gatsby-source-wordpress/src/normalize.js#L452))


## Credits

* [GatsbyJS](https://www.gatsbyjs.org/)
* [GatsbyJS Source Plugin tutorial](https://www.gatsbyjs.org/docs/source-plugin-tutorial/)