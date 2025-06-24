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
| phiên bản cũ và mới | [index-windows.json](https://www.python.org/ftp/python/index-windows.json) |
| Phiên bản cũ (Legacy) [index-windows-legacy.json](https://www.python.org/ftp/python/index-windows-legacy.json) |
| Phiên bản mới (Recent) [index-windows-recent.json](https://www.python.org/ftp/python/index-windows-recent.json) |

ví dụ php đọc mã nguồn và lọc version:
<?php
header('Content-Type: application/json');

function fetchPythonHtml() {
    $url = 'https://www.python.org/ftp/python/';

    $options = [
        "http" => [
            "method" => "GET",
            "header" => 
                "User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) Chrome/114.0.0.0 Safari/537.36\r\n" .
                "Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8\r\n" .
                "Referer: https://www.python.org/\r\n"
        ]
    ];

    $context = stream_context_create($options);

    $html = @file_get_contents($url, false, $context);

    if ($html === false) {
        http_response_code(500);
        echo json_encode(["error" => "Failed to fetch data from python.org"]);
        exit;
    }

    return $html;
}

function extractVersions($html) {
    $regex = '/href="(\d+\.\d+(?:\.\d+)?)\//';
    preg_match_all($regex, $html, $matches);

    if (!isset($matches[1])) return [];

    // Loại trùng và sắp xếp giảm dần
    $versions = array_unique($matches[1]);

    usort($versions, function($a, $b) {
        $va = explode('.', $a);
        $vb = explode('.', $b);
        for ($i = 0; $i < max(count($va), count($vb)); $i++) {
            $na = isset($va[$i]) ? (int)$va[$i] : 0;
            $nb = isset($vb[$i]) ? (int)$vb[$i] : 0;
            if ($na !== $nb) return $nb - $na;
        }
        return 0;
    });

    return $versions;
}

// Main logic
$html = fetchPythonHtml();
$versions = extractVersions($html);
echo json_encode($versions);
