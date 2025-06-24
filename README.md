# 🐍 Python Version Fetcher for Windows

Công cụ lấy danh sách các phiên bản Python dành cho hệ điều hành **Windows** từ trang chủ chính thức [python.org](https://www.python.org).

---

## 🔔 Cập nhật quan trọng

Trước đây, việc lấy danh sách phiên bản Python được thực hiện thông qua:

- Trang FTP: `https://www.python.org/ftp/python/`
- API trung gian (proxy cloud): `https://soft.721pc.workers.dev/?dev=python`

Tuy nhiên, **API proxy cloud hiện không còn hoạt động**, do bị chặn kết nối từ trang chủ Python.

---

## ✅ API mới được hỗ trợ

Trang chủ Python đã cung cấp các endpoint JSON chính thức để truy cập danh sách phiên bản dành cho Windows:

| Tên API | Link |
|---------|-------|------|
| phiên bản cũ và mới | [index-windows.json](https://www.python.org/ftp/python/index-windows.json)

| Phiên bản cũ (Legacy) [index-windows-legacy.json](https://www.python.org/ftp/python/index-windows-legacy.json)

| Phiên bản mới (Recent) [index-windows-recent.json](https://www.python.org/ftp/python/index-windows-recent.json)



