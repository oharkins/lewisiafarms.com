# Lewisia Farms - Hugo Website

This is the Hugo website for Lewisia Farms. This `README` outlines the steps to develop the site locally and publish it directly to an Amazon S3 bucket using Hugo's built-in deployment commands.

## Prerequisites

Before starting, ensure the following are installed:

- [Hugo](https://gohugo.io/getting-started/installing/)
- [AWS CLI](https://aws.amazon.com/cli/)
- Access to your Amazon S3 bucket

## Local Development

1. **Clone the repository:**

    ```bash
    git clone <repository-url>
    cd lewisia-farms
    ```

2. **Start the Hugo development server:**

   Run the server to preview changes locally. 
   
    ```bash
   hugo server -D --disableFastRender
   ```
   
   The `-D` flag includes any draft content.
   The `--disableFastRender` flag disables the fast render mode, which can cause issues with some content types in Hugo 0.6

   You can view the site at `http://localhost:1313`.

3. **Make changes:**

    - Modify or add content in the `content/` folder.
    - Update layouts or themes as needed.

4. **Stop the server:**

   Press `Ctrl + C` to stop the local development server when you're done.

## Configuring Deployment to S3

Before publishing to S3, ensure your Hugo configuration file (`config.toml`, `config.yaml`, or `config.json`) is set up with your deployment details.

1. **Add S3 deployment configuration to your Hugo config file** (typically `config.toml`):

    ```toml
    [deployment]
      [[deployment.targets]]
      name = "s3"
      URL = "s3://your-s3-bucket-name"
      cloudFrontDistributionID = "YOUR_CLOUDFRONT_DISTRIBUTION_ID" # Optional

      [deployment.targets.headers]
      Cache-Control = "max-age=31536000, no-transform, public"
      X-Content-Type-Options = "nosniff"
    ```

   Replace `your-s3-bucket-name` with your actual S3 bucket name, and add a CloudFront distribution ID if you are using CloudFront for content delivery.

2. **Configure AWS CLI credentials** (if not already done):

   Set up your AWS credentials by running:

    ```bash
    aws configure
    ```

   Provide your AWS Access Key, Secret Access Key, region, and output format.

## Building and Publishing the Site

1. **Build the site:**

   Build the site for production:

    ```bash
    hugo
    ```

   This will generate the static files in the `public/` folder.

2. **Deploy the site to S3:**

   Use Hugo's built-in deployment command:

    ```bash
    hugo deploy
    ```

   This command will sync your `public/` folder to your S3 bucket as specified in your Hugo configuration.

3. **Invalidate CloudFront cache (optional):**

   If you're using CloudFront, Hugo will invalidate the cache for you if you've provided the `cloudFrontDistributionID` in the configuration file. If you wish to manually invalidate the cache, you can use:

    ```bash
    aws cloudfront create-invalidation --distribution-id YOUR_CLOUDFRONT_DISTRIBUTION_ID --paths "/*"
    ```

## Site Configuration

You can adjust global settings in the `config.toml` file, such as:

- **Base URL**
- **Site title**
- **Permalinks**
- **Themes and layout settings**

## Additional Resources

- [Hugo Documentation](https://gohugo.io/documentation/)
- [AWS S3 Documentation](https://docs.aws.amazon.com/s3/index.html)
- [AWS CloudFront Documentation](https://docs.aws.amazon.com/cloudfront/index.html)

## License

This project is licensed under the MIT License.
