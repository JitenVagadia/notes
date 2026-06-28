# Amazon CloudFront
* CDN
* DDoS protection, Integration with Shield, AWS WAF
* CloudFront Origins
  * S3 bucket
    * Secured using OAC (Origin Access Control)
  * VPC Origin
  * Custom Origin
  * has a concept of Origin group: 1 primary 1 secondary incase primary not available
* CloudFront CacheKey
  * unique id for every object in cache, by default contains hostname + resource portion of url, can add other elements using cache policies
  * cache based on: headers, cookies, query strings
* Origin Request Policy
  * what to forward to origin without including it in cache key
  * can also add custom headers in request to origin, that were not there in the client request
* supports cache invalidations
* CloudFront Cache Behaviors
  * different behavior for different paths in url
    * different origin
* Good idea to separate static and dynamic distributions
  * dynamic -> cache based on headers, cookies
  * static -> no header/sesstion caching rules
* EC2/ALB as an Origin
  * (User)->(CloudFront)->(VPC Origin -> (EC2,ALB,etc.)inPrivateSubnet)inVPC
* CloudFront Geo Restriction
  * allow/deny list based on countries
  * countries determined by ip range (3rd party ip db)
* CloudFront signed URL/cookie
  * attach a policy with
    * expiration, trusted signer, ip ranges allowed
    * url -> single file, cookies -> multiple files
    * client -> some app for authNZ and giving back signed url/cookie, that app -> cloudFront (to generate signed)
  * cloudfront signed url and s3 signed urls are different
    * s3 -> issue request as a person to sign, IAM based: IAM key of the IAM Principal
    * cloudfront -> origin dont matter, account wide, can make to consider ip, path,date,exp
* Pricing
  * 3 classes: All -> all end locations included, 200 -> only cheapest 200, 100 -> only cheapest 100
* Field Level Encryption
  * edge location encrypts fields in payload based on configured public key
  * actual end app has the private key to decrypt that field
* CloudFront - Real time logs
  * cloudfront --logs-> (Kinesis Data streams) --records-> (lambda|kinesis data firehose)
  * can configure sampling rate, paths, fields