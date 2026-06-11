# Portfolio Website

A production-style portfolio website built with SvelteKit and deployed on AWS. The project focuses on a polished frontend experience, automated deployment, and cloud infrastructure practices that keep the site fast, secure, and reproducible.

## Overview

The frontend is built with SvelteKit, TypeScript, and Tailwind CSS, then deployed as a static site to Amazon S3. The site is served globally through Amazon CloudFront for HTTPS, edge caching, and low-latency content delivery. A custom domain is configured with DNS records and AWS Certificate Manager (ACM) to support secure TLS access.

Deployment is automated with GitHub Actions. Every push to the `release` branch installs dependencies, builds the SvelteKit application, syncs the generated static files to S3, and invalidates the CloudFront cache so visitors receive the latest version.

AWS access for deployment uses GitHub OpenID Connect (OIDC) federation. The workflow assumes an IAM role at deploy time instead of relying on long-lived AWS access keys, keeping credentials short-lived and scoped to the permissions needed for deployment.

Infrastructure configuration files for CloudFront, S3 bucket policy, IAM trust policy, and deployment permissions are kept in source control to document and reproduce the environment.

## Technologies

- SvelteKit
- TypeScript
- Tailwind CSS
- AWS S3
- AWS CloudFront
- AWS Certificate Manager (ACM)
- AWS IAM
- GitHub Actions
- GitHub OIDC Federation
- AWS CLI
- DNS Management
- Infrastructure as Code concepts

## Project Structure

```text
src/
  lib/components/       Reusable Svelte components
  routes/               SvelteKit routes and project detail pages
static/
  fonts/                Self-hosted font assets
  projects/             Project preview assets
infrastructure/         AWS policy and CloudFront configuration examples
.github/workflows/      GitHub Actions deployment workflow
```

## Local Development

Install dependencies:

```sh
npm install
```

Start the development server:

```sh
npm run dev
```

Run Svelte and TypeScript checks:

```sh
npm run check
```

Build the production site:

```sh
npm run build
```

Preview the production build locally:

```sh
npm run preview
```

## Deployment

Deployments are handled by `.github/workflows/deploy.yml`.

The workflow runs on pushes to the `release` branch and uses:

- `AWS_ROLE_ARN` stored as a GitHub Actions secret
- GitHub OIDC to assume the AWS IAM deployment role
- `aws s3 sync` to upload the static build output
- `aws cloudfront create-invalidation` to refresh cached content

No long-lived AWS access keys are stored in this repository.
