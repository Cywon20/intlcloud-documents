## Overview

This document describes how to create a file system and mount point on the file system page in the CFS console.

## Directions

### 1. Sign up for a Tencent Cloud account

If you already have a Tencent Cloud account, ignore this step.
<div style="background-color:#00A4FF; width: 320px; height: 35px; line-height:35px; text-align:center;"><a href="https://www.tencentcloud.com/en/account/register"_blank"  style="color: white; font-size:16px;">Click here to sign up for a Tencent Cloud account</a></div>

### 2. Enter the file system page

Log in to the [CFS console](https://console.cloud.tencent.com/cfs) and click **File System** on the left sidebar to enter the file system list page.

### 3. Determine the type of file system to create based on your business needs

For more information on CFS storage types, see [Storage Types and Performance](https://intl.cloud.tencent.com/document/product/582/33745).

### 4. Create a file system and mount target

1. Click **Create**.
2. Configure the following information on the **Create File System** page.
<table>
  <tr>
    <th nowrap="nowrap">Field</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Billing Mode</td>
    <td>Select the desired billing mode. Two billing modes are supported: pay-as-you-go and prepaid.</br><b>Note: </b>Only some products support the prepaid mode.</td>
  </tr>
	<tr>
    <td>File System Name</td>
    <td>Customize the name of the file system to create.</td>
  </tr>
  <tr>
    <td>Region</td>
    <td>Select the region where the CFS file system is to be created.</td>
  </tr>
  <tr>
    <td nowrap="nowrap">Availability Zone</td>
    <td>Select the AZ where the CFS file system is to be created.</td>
  </tr>
  <tr>
    <td nowrap="nowrap">Protocol</td>
    <td>Select a protocol type, NFS or SMB, for the file system. NFS is more suitable for Linux and Unix clients. CIFS/SMB is more suitable for Windows clients. The Turbo series can only be used by private clients and does not allow the selection of file system protocols.</td>
  </tr> 
  <tr>
    <td>Permission Group</td>
    <td>Each file system must be bound to a permission group. The permission group specifies an allowlist that can access the file system and lists the read and write permissions.
    </td>
  </tr>
	 <tr>
    <td>Storage Capacity</td>
    <td>Required only for the Turbo series. The Turbo series is an exclusive cluster and has restrictions on the minimum cluster scale and expansion step. For Standard Turbo, the minimum initial cluster scale is 40 TiB, and the expansion step is 20 TiB. For High-Performance Turbo, the minimum initial cluster scale is 20 TiB, and the expansion step is 10 TiB.
  </tr>
	 <tr>
    <td>CCN</td>
    <td>Required only for the Turbo series. Select an existing CCN or create a new one. For more information, see <a href="https://www.tencentcloud.com/products/ccn">Cloud Connect Network</a>. 
  </tr>
	<tr>
    <td>IP Range</td>
    <td>Required only for the Turbo series. This parameter allows you to reserve an IP range for Turbo related components. Ensure that the selected IP range does not conflict with the IP ranges of other instances on the cloud that need to communicate with Turbo. To ensure the number of IP addresses in the IP range, the mask must have 16 to 24 bits, for example, 10.0.0.0/24.
    </td>
	 </tr>
	<tr>
    <td>Tag</td>
    <td>
		<ul  style="margin: 0;">
      <li>If you already have a tag, you can add it to the new file system here.</li>
			<li>If you do not have a tag, log in to the <a href="https://console.cloud.tencent.com/tag/taglist">Tag console</a> to create a desired tag, and then bind it to the file system. You can also add a tag to the file system after the file system is created.</li></ul>
    </td>
  </tr>
</table>
3. Click **Buy Now**.
4. Click **Create Now** to create the file system and mount target.


### 5. Get the mount target information

1. After the file system is created, return to the file system list.
2. Click the name of the file system to enter its basic information page.
3. Click **Mount Target Info** to get the mount commands on Linux and Windows.
You are advised to copy the mount command provided by the console for mounting.




