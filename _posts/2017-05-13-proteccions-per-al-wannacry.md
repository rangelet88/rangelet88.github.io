---
layout: post
title: Proteccions per al WannaCry
date: 2017-05-13 12:32
author: rangel
comments: true
categories: [Apunts, malware, ransomware, seguretat, virus, wannacry, wannacrypt, windows]
visible: true
---
El passat divendres s'alliberava el virus de tipus ransomware conegut amb el nom de <b>WannaCry</b> o <b>WannaCrypt</b>, infectant més de 150.000 màquines en 74 països, causant el caos per tot el món i provocant que moltes empreses tinguessin que apagar els servidors i demanar als seus treballats que apaguessin els seus ordinadors, en la "ciber-infecció" més gran fins a la data després del virus "ILoveYou" al 2000.

Com la majoria de ransomwares, un cop s'ha executat xifrava tots els arxius i en demanava un rescat amb bitcoins per a recuperar les dades. La particularitat però, d'aquesta versió és la seva capacitat per a estendre's per la xarxa tot explotant una vulnerabilitat de Windows i infectar tots els ordinadors que la conformen.

En aquesta entrada, aniré publicant diferents <del>solucions</del> mesures preventives, per a fer front a l'atac del #WannaCry.

<!--more-->

<ul>
	<li><b>Còpies de seguretat</b></li>
Com que no existeix una solució mestra per a evitar la infecció, el pas més important serà protegir les nostres dades per a restablir tota la nostra informació, en cas que ens veiem compromesos pel virus.

<blockquote><b>Avís!</b> Degut a que aquest virus s'escampa per dintre de la nostra xarxa, és important que tinguem les nostres còpies aïllades i protegides (en discs externs, equips desconnectats de la xarxa o sense permís d'usuari) per a que no es vegin compromeses!</blockquote>

	<li><b>Actualitzar!</b></li>

Tot i que la font d'infecció encara no està clara, si que sabem que la propagació del virus per la xarxa utilitza una coneguda vulnerabilitat de SMB (<b><a href="https://technet.microsoft.com/en-us/library/security/ms17-010.aspx" target="_blank">MS17-010</a></b>) que Microsoft va arreglar el passat mes de març, i que moltes empreses encara no havien actualitzat.

Per tant, el primer pas a fer és actualitzar els nostres servidors i equips de sobretaula!

<li><b>Aplicar els "pedaços" per als Windows no suportats</b></li>

Degut al gran nombre d'equips que encara utilitzen versions anteriors a Windows 7, com ara Windows XP o Windows Server 2008; Microsoft excepcionalment ha creat uns "pedaços" per a arreglar la vulnerabilitat tot i que fa temps que havia anunciat que no en donaria suport.

Podem descarregar els paquets d'actualització i obtenir més informació al següent article: <a href="https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for-wannacrypt-attacks" target="_blank">https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for-wannacrypt-attacks</a>

	<li><b>Bloquejar ports als Firewall</b></li>

Per a evitar tant que el virus "truqui a casa" com es propagui per la nostra xarxa, haurem de bloquejar els següents ports del nostre firewall: <b>137, 138 UDP i 137, 445 TCP</b>. Alerta! amb això també estarem bloquejant tot el trànsit SMB.

	<li><b>Desinstal·lar de les característiques de Windows</b></li>

Haurem d'anar al Tauler de Control &gt; Programes i Característiques &gt; Activar o Desactivar les característiques de Windows. I desmarcar l'opció "<b>Compatibilidad con el protocolo para compartir archivos SMB1.0/CIFS</b>"

<p style="text-align: center"><a href="https://bloc.garcad.cat/wp-content/uploads/2017/05/wannacrypt1.png"><img src="https://bloc.garcad.cat/wp-content/uploads/2017/05/wannacrypt1-300x180.png" alt="wannacrypt#1" width="300" height="180" class="aligncenter size-medium wp-image-571" /></a></p>

	<li><b>Deshabilitar el servei SMBv1 des del registre:</b></li>

Haurem de modificar el registre de Windows i crear o modificar l'entrada SMB1 de la següent clau:

<blockquote><b>Nota:</b> abans de modificar el registre assegurem-nos que hem fet còpia i que podem restaurar-lo en cas que alguna cosa surti malament. Per a més informació, podeu consultar l'article <a href="https://support.microsoft.com/en-us/help/322756/how-to-back-up-and-restore-the-registry-in-windows" target="_blank">322756</a> de Microsoft on s'explica com fer-ho.</blockquote>

<b>HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\LanManServer\Parameters\</b>

Nom de l'entrada: <b>SMB1</b>
Tipus d'entrada: <b>REG_DWORD</b>
Possibles valors: <b>0 = Deshabilitat</b>, 1 = Habilitat (Per defecte, sense l'entrada, està habilitat)

	<li><b>Execució de l'script NoMoreCry</b></li>

El CCN-CERT ha desenvolupat una eina per prevenir l'encriptació de les dades en cas que s'arribi a executar el WannaCry.

L'script, de forma preventiva, el que fa es accedir a les carpetes personals de l'usuari on s'executa el virus i crear un arxiu buit amb el mateix nom que aquest ho faria "t.wcry" i un cop creat li esborra tots els permisos. D'aquesta forma quan el ransom vol crear l'arxiu per a fer el xifrat "t.wcry" es troba en que aquest ja existeix i com no té permisos no el pot sobreescriure, per tant, aquí finalitza la seva execució i les dades queden sense xifrar.

Font per a descarregar l'eina de la pàgina del CCN-CERT: <a href="https://loreto.ccn-cert.cni.es/index.php/s/tYxMah1T7x7FhND" target="_blank">https://loreto.ccn-cert.cni.es/index.php/s/tYxMah1T7x7FhND</a>

</ul>

Espero que us hagi pogut ajudar i que el #WannaCry no us afecti.
