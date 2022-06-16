# duvet-action

This action builds and installs [Duvet](https://github.com/aws/s2n-quic/tree/main/common/duvet), generates a compliance report via a provided script, and publishes the result to an S3 bucket.

# Usage

### `report-script: ''`

Path to a script that generates a Duvet report. See `duvet report --help` for more information about generating reports. The action expects `report.html` to be generated to the same directory the report generation script is contained in.

The script will be passed `github.sha` in the first argument, which can be used with the `--blob-link` Duvet argument.

### `aws-access-key-id: ''`

An AWS access key. The corresponding user must have S3 write permissions.

### `aws-secret-access-key: ''`

The AWS secret key.

### `aws-s3-bucket-name: ''`

The name of the destination S3 bucket which the report will be uploaded to.

### `cdn: ''`

An optional CDN which will prefix the published S3 URL in the `compliance / report` Github check.


## Example usage:

```yml
jobs:
  duvet:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: goatgoose/duvet-action@v1.0.0
        with:
          report-script: compliance/generate_report.sh
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-s3-bucket-name: s2n-tls-ci-artifacts
          aws-s3-region: us-west-2
          cdn: https://d3fqnyekunr9xg.cloudfront.net
```
