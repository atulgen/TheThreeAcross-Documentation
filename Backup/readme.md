## The3Across

### Frontend

- The codebase repo is maintained in Github itself is the back up. 

### Backend :

- Hosted in AWS
- Login to AWS


1. Strapi Self-Hosted (Local or Custom Server)

    Use the Strapi Data Export Command:
    Strapi 5 provides a CLI command to export your entire project data, including the database content, configuration, assets, and schemas.
    Example command:

```bash
yarn strapi export --file my-strapi-export
```

or

```bash
npm run strapi export -- --file my-strapi-export

```



By default, this creates an encrypted and compressed archive. You can disable encryption or compression with --no-encrypt and --no-compress if you want a plain .tar file.


The exported archive will include:

    Project configuration
    All content (entities)
    Relations (links)
    Assets (files in uploads)
    Schemas
    Metadata
    Note: Admin users and API tokens are not exported. Media from third-party providers are not included in the export.
    See more details in the official documentation.


#### Importing:

And for importing 
To import a full backup into your Strapi backend, use the strapi import command. This command restores your content, configuration, assets, and schemas from an archive created by strapi export. Here’s how to do it:
Basic Import Command

```bash
yarn strapi import -f /path/to/your/export_file.tar.gz.enc
```

