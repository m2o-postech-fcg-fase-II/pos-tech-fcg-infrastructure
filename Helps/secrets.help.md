cd# Na pasta onde fica o docker-compose.yml

# Execute a seguinte instrução para criar e gerar uma secret
New-Item -ItemType Directory -Force -Path .\secrets
[byte[]] $b = New-Object byte[] 32; (New-Object System.Security.Cryptography.RNGCryptoServiceProvider).GetBytes($b)
[Convert]::ToBase64String($b) | Set-Content -Path .\secrets\jwt_secret -NoNewline
icacls .\secrets\jwt_secret /inheritance:r /grant:r "$($env:USERNAME):(R)"