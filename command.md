$repo = (& git -C (Get-Location).Path rev-parse --show-toplevel 2>$null)

if ($LASTEXITCODE -ne 0 -or -not $repo) {
    $repoPath = Read-Host "Nhap duong dan Git repo"
    $repo = (& git -C "$repoPath" rev-parse --show-toplevel 2>$null)
}

if ($LASTEXITCODE -ne 0 -or -not $repo) {
    Write-Host "Khong tim thay Git repo." -ForegroundColor Red
    return
}

$repo = $repo.Trim()

Write-Host ""
Write-Host "Chon tai khoan cho repo hien tai:"
Write-Host "1. Git ca nhan"
Write-Host "2. Git cong ty"
$choice = Read-Host "Nhap 1 hoac 2"

switch ($choice) {
    "1" {
        $name = "Duc Trinh"
        $email = "hoangductrinh2k5@gmail.com"
        $label = "ca nhan"
    }
    "2" {
        $name = "Trinhhd"
        $email = "trinhhd@kaopiz.com"
        $label = "cong ty"
    }
    default {
        Write-Host "Lua chon khong hop le. Chua thay doi gi."
        return
    }
}

git -C "$repo" config --local user.name "$name"
if ($LASTEXITCODE -ne 0) {
    Write-Host "Khong doi duoc ten Git." -ForegroundColor Red
    return
}

git -C "$repo" config --local user.email "$email"
if ($LASTEXITCODE -ne 0) {
    Write-Host "Khong doi duoc email Git." -ForegroundColor Red
    return
}

Write-Host ""
Write-Host "Da chon Git $label cho repo:"
Write-Host $repo
Write-Host "Email commit: $email"

$remote = git -C "$repo" remote get-url origin 2>$null
if ($remote) {
    Write-Host "Remote origin: $remote"
}