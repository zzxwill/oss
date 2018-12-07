# FAQs about subaccount settings {#concept_ktx_ktz_5db .concept}

## How to create an STS temporary account and how to use it to access resources? {#section_w1h_ntz_5db .section}

See [STS temporary access authorization](reseller.en-US/Best Practices/Access control/STS temporary access authorization.md#).

## Client or console logon error reported for an authorized sub-account {#section_edc_4tz_5db .section}

See [Why does a sub-account encounters an error of no operation permission for a bucket on the OSS console after it has been granted the bucket operation permission](https://partners-intl.aliyun.com/help/doc-detail/58905.htm).

## How to authorize a sub-account with the operation permission for a single bucket {#section_xxv_4tz_5db .section}

See [How to assign the full operation permission for a specified bucket to a sub-account](https://partners-intl.aliyun.com/help/doc-detail/58905.htm).

## How to authorize a sub-account with the operation permission for a directory in a bucket {#section_brw_ptz_5db .section}

See [OSS directory authorization](https://partners-intl.aliyun.com/help/doc-detail/58905.htm).

## How to authorize a sub-account with the read-only permission for a bucket {#section_chl_qtz_5db .section}

See [Authorize a sub-user to list and read resources in a bucket](https://partners-intl.aliyun.com/help/doc-detail/58905.htm).

## Error upon an OSS SDK call: InvalidAccessKeyId {#section_y12_rtz_5db .section}

See [STS errors and troubleshooting](../../../../reseller.en-US/Errors and Troubleshooting/STS common errors and troubleshooting.md#).

## Error upon an STS call: Access denied by authorizer’s policy {#section_kt5_rtz_5db .section}

Detailed error information: ErrorCode: AccessDenied ErrorMessage: Access denied by authorizer’s policy.

Cause of the error:

-   The temporary account has no access permission.
-   The authorization policy specified for assuming the role of this temporary account does not assign the access permission to the account.

For more STS errors and the causes, see [OSS permission errors and troubleshooting.](../../../../reseller.en-US/Errors and Troubleshooting/OSS permission.md#)

