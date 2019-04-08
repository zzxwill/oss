# FAQ {#concept_vsh_vc2_ghb .concept}

This topic answers the questions that are frequently asked in the use the compliant retention strategy for Alibaba Cloud OSS resources.

## What are the advantages of the compliant retention strategy? {#section_uf5_gmf_hhb .section}

The compliant retention strategy ensures that data is stored in a manner conforming to the compliance requirements of industry regulations. Data protected by a compliance retention strategy cannot be modified or deleted by any user within the protection period. However, if you use RAM policies or bucket policies to protect the data, it may still be modified or deleted.

## When should I set a compliant retention strategy? {#section_jl3_5sy_ghb .section}

If you want to store important data that is not allow to modify or delete \(such as medical records, technical documentations, and contracts\) for a long period, you can store the data in a bucket and set a compliant retention strategy for the bucket to protect the data.

## Can I set a compliant retention strategy for an object? {#section_rwp_3mf_hhb .section}

Currently, you can set a compliant retention strategy only for a bucket but not a directory or an object.

## Can I modify the storage class of an object after setting a compliant retention strategy for the bucket that stores the object? {#section_u1t_jmf_hhb .section}

After setting a compliant retention strategy for a bucket, you can [set lifecycle rules](reseller.en-US/Developer Guide/Manage files/Manage lifecycle rules.md#) to convert the storage class of the objects in the protected bucket to save storage costs.

## How do I delete a bucket for which a compliant retention strategy is set? {#section_yp5_lmf_hhb .section}

-   You can directly delete the bucket if no objects are stored in it.
-   If objects that are out of the protection period are stored in the bucket, a failure occurs when you try to delete the bucket. In this case, you can delete the objects in the bucket first and then delete the bucket.
-   If objects that are in the protection period are stored in the bucket, you cannot delete the bucket.

## Are my objects within the protection period of a compliant retention strategy kept when my account has overdue bills? {#section_yfq_mmf_hhb .section}

If your account has overdue bills, Alibaba Cloud applies data retention strategies to your data based on the terms and conditions in your contract.

## Can I set a compliant retention strategy as an authorized RAM user? {#section_vk2_nmf_hhb .section}

APIs related to the compliant retention strategy are open and integrated into RAM policies. By using a RAM user authorized by RAM policies, you can create or delete a compliant retention strategy in the console or through the APIs and SDKs.

