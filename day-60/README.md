# Day 60: Recommender API

A FastAPI exercise serving content, SVD, hybrid and BPR recommendations. This folder contains the application, CSV data, saved model artifacts and a Dockerfile.

## Current setup status

The Dockerfile copies `day-60/requirements.txt`, but that file is not committed. A clean clone therefore cannot build the image as written. The saved pickle artifacts also depend on the library versions used to train them; those versions are not recorded. Restore and validate that environment before treating this as a reproducible deployment.

The source loads `data/` and `models/` relative to its working directory and deserializes model files at startup. Run it only with trusted model artifacts. Once a compatible dependency file has been recovered and installed, the local entry point is:

```bash
# From the repository root, in the prepared Python environment:
cd day-60
python -m uvicorn app:app --host 127.0.0.1 --port 8000
```

This is the intended invocation, not a newly verified runtime result. There is no database service in this implementation.

## Routes

| Route | Purpose |
| --- | --- |
| `/health` | Health response after application startup |
| `/docs` | FastAPI's interactive API documentation |
| `/recommend/content/{track_id}` | Content-based recommendations |
| `/recommend/cf/{user_id}` | SVD recommendations |
| `/recommend/hybrid/{user_id}` | Hybrid recommendations |
| `/recommend/bpr/{user_id}` | BPR recommendations |

## Docker build after restoring requirements

From the repository root, use the root build context because the Dockerfile's `COPY` paths start with `day-60/`:

```bash
docker build -t lsp60-recommender:day60 -f day-60/Dockerfile .
docker run --rm -p 127.0.0.1:8000:8000 lsp60-recommender:day60
```

The commands remain blocked until the missing requirements and model compatibility are resolved. Training-data suitability, authentication, performance and production deployment have not been certified by this documentation update.

Return to the [milestone index](../README.md) for the earlier training and ranking exercises. The original exercise was published under the `xenon6230/recommender-api` Docker Hub name; the current source's reproducibility limits above still apply.
