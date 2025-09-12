# Gather OCI Data from the Console

This guide walks you through collecting all required Oracle Cloud Infrastructure (OCI) details for your setup.

---

## 1. Log in to the OCI Console
- Go to the [OCI Console](https://cloud.oracle.com).
- If you don’t have an account, sign up for a **Free Tier account**.

---

## 2. Tenancy OCID & Region
1. In the top-right corner, click your **Profile icon** (initials or avatar).
2. Select **Tenancy: <your tenancy name>** from the dropdown.
3. On the **Tenancy details** page:
   - Copy the **Tenancy OCID** (starts with `ocid1.tenancy.oc1..`).
   - Note your **Region** (top-right, e.g., `US East (Ashburn)` or `us-ashburn-1`).  
     Use the **region identifier** (e.g., `us-ashburn-1`) in your config.

---

## 3. User OCID
1. From the **Profile icon**, select **User Settings**.
2. On the **User details** page, copy the **User OCID** (starts with `ocid1.user.oc1..`).

---

## 4. API Key & Fingerprint
1. In **User Settings**, go to **API Keys** (left sidebar).
2. Click **Add API Key**.
3. Choose:
   - **Generate API Key Pair** (recommended), or  
   - **Paste Public Key**.
4. Download the **private key** (e.g., `oci_api_key.pem`) and **public key**.  
   Store the private key securely in `~/.oci/` and set correct permissions:
   ```bash
   chmod 600 ~/.oci/oci_api_key.pem
   ```
5. Upload the public key.  
   OCI will display the **Fingerprint** (e.g., `xx:xx:xx:...`). Copy it.
6. Note the full **private key path** (e.g., `/home/user/.oci/oci_api_key.pem`).

---

## 5. Compartment OCID
1. Navigate to **Identity & Security > Compartments**.
2. Select your **root compartment** or create a new one:
   - Click **Create Compartment** and name it (e.g., `AlwaysFreeSetup`).
3. Open the compartment details and copy the **Compartment OCID**  
   (starts with `ocid1.compartment.oc1..`).

---

## 6. Ubuntu 22.04 Image OCID (Region-Specific)
1. Go to **Compute > Instances**.
2. Click **Create Instance** (no need to complete the process).
3. Under **Image and Shape**, click **Edit**.
4. Select:
   - **OS:** Ubuntu
   - **Version:** Canonical Ubuntu 22.04 (standard or minimal)  
     *(must be ARM-compatible for Always Free Ampere instances)*.
5. Click the **info (ℹ️)** icon next to the image.
6. Copy the **Image OCID** (starts with `ocid1.image.oc1..`).

---

## 7. Example Configuration
Once you’ve gathered all the required information, your configuration should look like this:

```hcl
tenancy_ocid      = "ocid1.tenancy.oc1..example"
user_ocid         = "ocid1.user.oc1..example"
fingerprint       = "xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx"
private_key_path  = "/path/to/.oci/oci_api_key.pem"
compartment_ocid  = "ocid1.compartment.oc1..example"
region            = "us-ashburn-1"
ssh_public_key    = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQD... user@example.com" # full content of id_rsa.pub
ubuntu_image_ocid = "ocid1.image.oc1..example-for-ubuntu-22.04"
```

---
