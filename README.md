# Svazer Upload

File upload service using Vercel Blob storage. Files are stored in Vercel Blob CDN and served via vanity URL.

## Stack

- **Runtime**: Vercel Serverless (Node.js)
- **Storage**: Vercel Blob (CDN)
- **UI**: Static HTML + vanilla JS
- **File handling**: busboy (multipart parser)

## Deploy to Vercel

### 1. Create Vercel Blob Store

1. Go to [Vercel Dashboard → Storage](https://vercel.com/dashboard/stores)
2. Click **Create Database** → select **Blob**
3. Name it (e.g. `svazer-upload`)
4. Choose nearest region
5. Copy `BLOB_READ_WRITE_TOKEN`

### 2. Deploy

```bash
npm install
npx vercel --prod
```

### 3. Set Environment Variable

In [Vercel Dashboard → Project → Settings → Environment Variables](https://vercel.com/dashboard):

| Name | Value |
|------|-------|
| `BLOB_READ_WRITE_TOKEN` | `vercel_blob_rw_xxxxxxxx` |

Add to **Production** environment, then redeploy:

```bash
npx vercel --prod
```

## Local Development

```bash
# Install dependencies
npm install

# Set env vars in .env
BLOB_READ_WRITE_TOKEN=vercel_blob_rw_xxxxxxxx

# Start server
npm start
```

## API

### POST /api/upload

Upload a file (multipart form-data, field: `file`).

Response:
```json
{
  "success": true,
  "url": "https://cdn-svazer.vercel.app/file/a1b2c3d4.jpg",
  "blobUrl": "https://xxxx.public.blob.vercel-storage.com/a1b2c3d4.jpg",
  "filename": "a1b2c3d4.jpg",
  "size": 123456,
  "mimetype": "image/jpeg"
}
```

### GET /file/:filename

Serve file directly from project domain (proxied from Blob).

## File Limits

| Limit | Value |
|-------|-------|
| Max file size | 25 MB |
| Vercel free storage | 10 GB |
| Vercel free bandwidth | 100 GB/month |

## Project Structure

```
svazer-upload/
├── api/
│   └── index.js         Vercel serverless function
├── public/
│   ├── index.html       Frontend
│   └── favicon.png
├── server.js            Local dev server
├── vercel.json          Vercel configuration
├── package.json
├── .env.example
└── README.md
```
