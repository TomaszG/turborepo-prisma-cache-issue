# Turborepo Prisma Client cache issue

# Preconditions

- Node.js 18.13.0
- pnpm 7.25.0
- zstd

## Repro steps
1. Install dependencies with
    ```bash
    pnpm i
    ```
2. Generate Prisma Client with
    ```bash
    pnpm turbo generate
    ```
3. Check if Prisma Client files are generated with 
    ```bash
    ls -la node_modules/.pnpm/@prisma+client*/node_modules/.prisma/client/**
    ```
4. Remove generated Prisma Client files with
    ```bash
    rm -rf node_modules/.pnpm/@prisma+client*/node_modules/.prisma/client/
    ```
5. Confirm that files were removed with 
    ```bash
    ls -la node_modules/.pnpm/@prisma+client*/node_modules/.prisma/client/**
    ```
    Should return **no matches found** error
6. Once again generate Prisma Client with 
    ```bash
    pnpm turbo generate
    ```
    Should restore generated client from turbo cache
7. Check if Prisma Client files were restored with 
    ```bash
    ls -la node_modules/.pnpm/@prisma+client*/node_modules/.prisma/client/**
    ```
    Files were not restored
8. Check if Prisma Client files were cached with
    ```bash
    tar --use-compress-program=unzstd -ztvf node_modules/.cache/turbo/27dc189e81124d92.tar.zst
    ```
    (hash may be different) - files are there but they were not restored