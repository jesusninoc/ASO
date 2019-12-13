```Powershell
##Server
while(1){
$port=2020
$endpoint = new-object System.Net.IPEndPoint ([IPAddress]::Any,$port)
$udpclient=new-Object System.Net.Sockets.UdpClient $port
$content=$udpclient.Receive([ref]$endpoint)
$udpclient.Close()
Invoke-Expression ([Text.Encoding]::ASCII.GetString($content))
}
#########################################################################
#Client
function menu_sistema
{
param (
[string]$Title = 'Menú Del Sistema'
)
cls
Write-Host "================ $Title ================"
 
Write-Host "1: Listar nombres de usuarios"
Write-Host "2: Crear usuarios"
Write-Host "3: Listar procesos"
Write-Host "4: Presiona 'S' para salir."
}

do
{
menu_sistema
$input = Read-Host "Selecciona una opción"
switch ($input)
{
'1' {
cls
'Ha seleccionado la opcion: Listar nombres de usuarios'
Write-Host
'-->A continuación se le mostrará en el Servidor una lista con los usuarios del sistema'
Write-Host
$port=2020
$endpoint = new-object System.Net.IPEndPoint ([IPAddress]"192.168.100.1",$port)
$udpclient=new-Object System.Net.Sockets.UdpClient
$b=[Text.Encoding]::ASCII.GetBytes('get-localuser')
$bytesSent=$udpclient.Send($b,$b.length,$endpoint)
$udpclient.Close()
}

 '2' {
cls
'Ha seleccionado la opción: Crear usuarios'
Write-Host
'-->A continuación se creara los usuarios en el servidor'
$port=2020
$endpoint = new-object System.Net.IPEndPoint ([IPAddress]"192.168.100.1",$port)
$udpclient=new-Object System.Net.Sockets.UdpClient
$b=[Text.Encoding]::ASCII.GetBytes(
'foreach($usuarios in gc .\usuarios.txt){
 $pass=ConvertTo-SecureString "11234aaa@@€dsf" -asplaintext -force
 $usuario = New-LocalUser $usuarios -Password $pass}'
     )
$bytesSent=$udpclient.Send($b,$b.length,$endpoint)
$udpclient.Close()


} 

'3' {
cls
Write-Host
'-->Para ver esta opción vaya al server'
$port=2020
$endpoint = new-object System.Net.IPEndPoint ([IPAddress]"192.168.100.1",$port)
$udpclient=new-Object System.Net.Sockets.UdpClient
$b=[Text.Encoding]::ASCII.GetBytes('get-process')
$bytesSent=$udpclient.Send($b,$b.length,$endpoint)
$udpclient.Close()


}

's' {
return
}
}
pause
}
until ($input -eq 's')
´´´
