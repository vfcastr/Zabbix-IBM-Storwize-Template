# 📡 Zabbix Template - IBM Storwize via SSH

Template for monitoring IBM Storwize (v3700, v5000, v7000, SVC) via SSH in Zabbix 7+ environments.

> ✅ Designed for production environments using a read-only user.

---

## 🔍 Overview

This template collects key storage metrics directly via Storwize CLI over SSH, converts output to JSON, and makes it available to Zabbix.

---

## 🧠 How IBM Storwize Works

IBM Storwize uses physical drives grouped into arrays with RAID (RAID-5/6/10), which form MDisks. MDisks are assigned to storage pools, from which logical volumes (vDisks) are created and exposed to hosts. The system is managed through two or more node canisters that monitor hardware components like batteries, PSUs, drives, fans, and internal links.

---

## 🛠️ Collected Data

| SSH Item                          | Description                                                              |
|----------------------------------|---------------------------------------------------------------------------|
| `Get storwize stats`             | General system stats (`lsvdisk`, `lsmdisk`, `lsdrive`, `lssystem`, etc.)  |
| `Get vdisk`                      | Detailed logical volume info                                              |
| `Get mdisk`                      | Physical disk group (MDisk) info                                          |
| `Get array`                      | Physical RAID arrays                                                      |
| `Get drive`                      | Physical drives                                                           |
| `Get host`                       | Connected hosts                                                           |
| `Get enclosure / psu / battery`  | PSU and battery status                                                    |
| `Get nodecanister`               | Physical controllers                                                      |
| `Get system / systemstats`       | System-wide metrics, cache, compression, usage                            |
| `Get quorum`                     | Quorum disk information                                                   |
| `Get eventlog`                   | Event log entries from the last 10 minutes                                |

---

## 📊 Key Metrics (Examples)

| Metric                             | Command/Source     | Description                                          |
|-----------------------------------|---------------------|------------------------------------------------------|
| `total_mdisk_capacity`            | `lssystem`          | Total MDisk capacity                                 |
| `space_allocated_to_vdisks`       | `lssystem`          | Allocated space to volumes                           |
| `total_free_space`                | `lssystem`          | Free storage space                                   |
| `vdisk.status`                    | `lsvdisk`           | Status of each vDisk                                 |
| `mdisk.status`                    | `lsmdisk`           | Status of each MDisk                                 |
| `enclosure.psu_status`            | `lsenclosurepsu`    | Status of enclosure power supplies                   |
| `compression_active`              | `lssystemstats`     | Whether compression is enabled                       |
| `eventlog.error_code`             | `lseventlog`        | Last critical alerts and errors                      |

---

## 🧾 Zabbix Pre-processing Logic

- SSH master item executes the full command (with `-delim :`)
- Dependent items extract metrics using JSONPath and regular expressions
- Outputs are split by sections using markers like `##BEGIN:<cmd>` and `##END:<cmd>`
- CSV to JSON conversion simplifies parsing of CLI outputs

---

## 📎 Requirements

- Zabbix 6 or later
- IBM Storwize user with read-only permissions
- SSH user/password

---

## ✅ Template Status

- [x] All core items implemented
- [x] Modular structure by CLI command
- [x] JSONPath and regex pre-processing

---

## 📌 Future Improvements

- Provide a sample Grafana dashboard

---
