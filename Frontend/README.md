# Frontend: TheThreeAcross


## Repository:

- Clone from https://github.com/atulraj85/the_three_across_frontend


Using SSH

```bash
git clone git@github.com:atulraj85/the_three_across_frontend.git
```





## Tech Stack used:

1. Nextjs 14
2. React 18
3. Shadcn
4. Nodemailer
5. Typescript
6. REST API

## Environment Variable requirements:

1. Nodemailer details:

```
EMAIL_USERNAME_FROM=
EMAIL_PASSWORD=
EMAIL_TO=
EMAIL_HOST=
EMAIL_SERVER_PORT=
```

2. Strapi API details:

```
NEXT_PUBLIC_STRAPI_API_TOKEN=your-api-token
NEXT_PUBLIC_PAGE_LIMIT=6
NEXT_PUBLIC_STRAPI_API_URL=http://localhost:1337
NEXT_PUBLIC_STRAPI_HOST=localhost
NEXT_PUBLIC_STRAPI_PROTOCOL=

```


### Example env:

```
NEXT_PUBLIC_STRAPI_API_TOKEN=10273dcc4a63431ced0baacfbac33a7c05edaa5c4b74a4e6ff29bef7c52e3a5237f1bd585ddf6e3b916c9666173b76f67094660cc6889bfa57d526ed5df102c6dc95631669111d8273321cf7a8e3ebe4c00d0a2ae550cf9e0b58f5e054db19acd64afa5266a313f988de6bed0aeda1b33a026178b3e266401ba850aa95cc2b79
NEXT_PUBLIC_PAGE_LIMIT=6
NEXT_PUBLIC_STRAPI_API_URL=http://localhost:1337
NEXT_PUBLIC_STRAPI_HOST=localhost
NEXT_PUBLIC_STRAPI_PROTOCOL=http

EMAIL_USERNAME_FROM=
EMAIL_PASSWORD=
EMAIL_TO=
EMAIL_HOST=
EMAIL_SERVER_PORT=
```





## Development:

1. Install all the packages:

```bash
yarn
```

2. Run the development server:

```bash
yarn dev
```




