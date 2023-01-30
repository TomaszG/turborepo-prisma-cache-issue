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
    **Expected result**: Should return **no matches found** error
6. Once again generate Prisma Client with 
    ```bash
    pnpm turbo generate
    ```
    **Expected result**: Should restore generated client from turbo cache
7. Check if Prisma Client files were restored with 
    ```bash
    ls -la node_modules/.pnpm/@prisma+client*/node_modules/.prisma/client/**
    ```
    **Result**: Files were not restored
8. Check if Prisma Client files were cached with (hashin the fileanme may be different)
    ```bash
    tar --use-compress-program=unzstd -ztvf node_modules/.cache/turbo/27dc189e81124d92.tar.zst
    ```
    **Result**: Files are there but they were not restored
