# Lab 3 Submission: Group 3

* **Client:** A jeepney fleet tracking service
* **VPC CIDR:** 172.23.48.0/20
* **Total VPC Addresses:** 4,096 addresses (172.23.48.0 to 172.23.63.255)

---

## Task 1. Divide the VPC into 8 Equal Subnets

1. **New Prefix:** /23 (20 + 3 extra bits)
2. **Subnet Table:**

| # | CIDR Block | First Address | Last Address | Usable | First Usable | Last Usable |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **1** | 172.23.48.0/23 | 172.23.48.0 | 172.23.49.255 | 507 | 172.23.48.4 | 172.23.49.254 |
| **2** | 172.23.50.0/23 | 172.23.50.0 | 172.23.51.255 | 507 | 172.23.50.4 | 172.23.51.254 |
| **3** | 172.23.52.0/23 | 172.23.52.0 | 172.23.53.255 | 507 | 172.23.52.4 | 172.23.53.254 |
| **4** | 172.23.54.0/23 | 172.23.54.0 | 172.23.55.255 | 507 | 172.23.54.4 | 172.23.55.254 |
| **5** | 172.23.56.0/23 | 172.23.56.0 | 172.23.57.255 | 507 | 172.23.56.4 | 172.23.57.254 |
| **6** | 172.23.58.0/23 | 172.23.58.0 | 172.23.59.255 | 507 | 172.23.58.4 | 172.23.59.254 |
| **7** | 172.23.60.0/23 | 172.23.60.0 | 172.23.61.255 | 507 | 172.23.60.4 | 172.23.61.254 |
| **8** | 172.23.62.0/23 | 172.23.62.0 | 172.23.63.255 | 507 | 172.23.62.4 | 172.23.63.254 |

3. **Five Reserved Addresses for Subnet 1:**
   * `172.23.48.0` — Network address
   * `172.23.48.1` — VPC router
   * `172.23.48.2` — DNS server
   * `172.23.48.3` — Kept by AWS for future use
   * `172.23.49.255` — Network broadcast address

---

## Task 2. Subnets of Size /25

* **2a.** 32 subnets
* **2b.** 123 usable addresses
* **2c. Subnet 12 CIDR:** `172.23.53.128/25` (Usable Range: `172.23.53.132` to `172.23.53.254`)
* **2d. Last Subnet CIDR:** `172.23.63.128/25`

---

## Task 3. Size Subnets from Host Counts

| Subnet Name | Prefix | CIDR Block | Availability Zone | Usable Addresses |
| :--- | :--- | :--- | :--- | :--- |
| `private-a` | `/22` | `172.23.48.0/22` | `ap-southeast-1a` | 1,019 |
| `private-b` | `/22` | `172.23.52.0/22` | `ap-southeast-1b` | 1,019 |
| `public-a` | `/25` | `172.23.56.0/25` | `ap-southeast-1a` | 123 |
| `public-b` | `/25` | `172.23.56.128/25` | `ap-southeast-1b` | 123 |

---

## Task 4. Check Proposed Subnets

* **P1 (`172.23.54.0/24`):** **Not valid**. It overlaps with `private-b` (`172.23.52.0/22`, range `172.23.52.0` to `172.23.55.255`) from Task 3.
* **P2 (`172.23.58.0/30`):** **Not valid**. A `/30` prefix is smaller than `/28`, which is the minimum subnet size supported by AWS.
* **P3 (`172.23.63.0/25`):** **Valid**. It is inside the VPC CIDR block, starts on a step boundary, is between `/16` and `/28`, and does not overlap Task 3 subnets.
* **P4 (`172.23.64.0/22`):** **Not valid**. It falls completely outside the VPC range, which ends at `172.23.63.255`.
