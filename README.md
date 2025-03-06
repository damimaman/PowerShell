# Power-Shell
Alterando permissão de pastas em massa, com script em power shell.
Abra o PowerShell como Administrador.

# Defina o caminho do diretório
$diretorio = "\\tasy_docs\gestaodecontratos\ans"

# Grupos que precisam de acesso total
$grupos = @("files_custos", "files_financeiro")

# Definir as permissões (Controle Total)
$permissao = "FullControl"

# Obter a ACL (Access Control List) do diretório
$acl = Get-Acl -Path $diretorio

# Adicionar permissões para cada grupo
foreach ($grupo in $grupos) {
    # Criar a regra de permissão para o grupo
    $regra = New-Object System.Security.AccessControl.FileSystemAccessRule(
        $grupo, 
        $permissao, 
        "ContainerInherit,ObjectInherit", 
        "None", 
        "Allow"
    )
    
    # Adicionar a regra à ACL
    $acl.AddAccessRule($regra)
}

# Aplicar a ACL modificada de volta ao diretório
Set-Acl -Path $diretorio -AclObject $acl

Write-Host "Permissões de Full Control foram atribuídas aos grupos $($grupos -join ', ') no diretório $diretorio."
