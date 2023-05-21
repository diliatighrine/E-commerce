Mini boutique en ligne et  devis par mail transmission individuelle des articles dans un panier paypal------------------------------------------------------------------------------------------------------
Url     : http://codes-sources.commentcamarche.net/source/47116-mini-boutique-en-ligne-et-devis-par-mail-transmission-individuelle-des-articles-dans-un-panier-paypalAuteur  : Tom_CrazyDate    : 01/08/2013
Licence :
=========

Ce document intitulé « Mini boutique en ligne et  devis par mail transmission individuelle des articles dans un panier paypal » issu de CommentCaMarche
(codes-sources.commentcamarche.net) est mis à disposition sous les termes de
la licence Creative Commons. Vous pouvez copier, modifier des copies de cette
source, dans les conditions fixées par la licence, tant que cette note
apparaît clairement.

Description :
=============

En une seule page:
<br />
<br /> on affiche les produits stock&eacute;s dans u
ne base de donn&eacute;es (prix, d&eacute;signations, frais de livraison/traitem
ent oui ou non...).
<br />
<br /> On renseigne le nombre de produits choisis.

<br />
<br /> Permet trois choix d'affichage ( uniquement les produits choisis
, non choisis ou les deux).
<br />
<br /> Permet une demande de devis par un f
ormulaire optionnel (nom email message) et transmet la demande &agrave; une adre
sse pr&eacute;d&eacute;finie et a celle du client (v&eacute;rification sommaire 
de la validit&eacute; de cette adresse).
<br />
<br /> Permet une impression s
&eacute;lective du devis.
<br />
<br /> Transmet les produits choisis &agrave;
 un panier PayPal business en d&eacute;taillant chaque produit ( quantit&eacute;
 prix)
<br />
<br /> Permet de diff&eacute;rencier les produits soumis &agrave
; des frais de livraison / traitement de ceux qui ne le sont pas.
<br /><a name
='source-exemple'></a><h2> Source / Exemple : </h2>
<br /><pre class='code' da
ta-mode='basic'>
&lt;!-- 
contenu du ficher de connexion db
&lt;?php //$conne
xion = mysql_connect('adresse serveur mysql','utilisateur','mot de passe') OR di
e(mysql_error());;
//mysql_select_db('nom de la base de donnees',$connexion);

?&gt;
--&gt;

&lt;!-- 
La creation et le remplissage des tables de la db ne 
sont pas gere ici
vous devez le faire manuellement
Le code du bouton d'envoi a
 PayPal provient du site PayPal, j'ai juste ajoute la boucle PHP pour gerer le m
ulti produits
--&gt;

&lt;form method=&quot;post&quot; name=&quot;facture&quo
t; action=&quot;&lt;?php echo $PHP_SELF.&quot;#bouton&quot;; ?&gt;&quot;&gt;
		
	&lt;input type='submit' name='action' value='Calculer'&gt;
			&lt;table class=
&quot;t1&quot; summary=&quot;Formulaire devis facture&quot;&gt;
				&lt;tr&gt;

					&lt;td class=&quot;c4&quot; style=&quot;width : 50%;&quot; &gt;Type de pre
station&lt;/td&gt;
					&lt;td class=&quot;c4&quot; style=&quot;width : 17%;&qu
ot; &gt;Quantités&lt;/td&gt;
					&lt;td class=&quot;c4&quot; style=&quot;width
 : 33%;&quot; &gt;Total ligne&lt;/td&gt;
				&lt;/tr&gt;

					&lt;!-- debut 
script php --&gt;
					&lt;?php
					  echo &quot;\n\n\n\n\n\n&lt;!--\n
					
  ##############################################################################
###############\n
					  # script de panier PayPal avec stockage des produits m
ysql et envoi du detail au panier PayPal\n
					  # gestion des frais de traite
ment livraison\n
					  # demande de devis possible avec envoi par mail au dema
ndeur et au gestionaire du site\n
					  ######################################
#######################################################\n\n
					  ############
################################################################################
#\n
					  # Ce script a ete developpe par 2AIDE, vous vous sera fourni gratuit
ement sur simple demande depuis le site <a href='http://www.2aide.fr/' target='_
blank'>http://www.2aide.fr/</a>\n
					  # Aucun support supplementaire ne vous
 sera propose gratuitement\n
					  # Vous pouvez malgre tout solliciter une ai
de dans le cadre des prestations proposees sur le site <a href='http://www.2aide
.fr/' target='_blank'>http://www.2aide.fr/</a>\n
					  # Bien qu'il en soit ma
intenant tres eloigne ce script a ete inspire, au depart par un formulaire de co
ntact propose gratuitement lui aussi.\n
					  # 2aide remercie donc <a href='h
ttp://www.patrickhamy.net/' target='_blank'>http://www.patrickhamy.net/</a> aute
ur de ce formulaire de contact\n
					  #######################################
######################################################\n\n
					  #############
################################################################################
\n
					  # A vous de voir pour les css\n
					  # Je precise juste que la cla
sse masimp est declaree dans une feuille de style dediee a l'impression: media=p
rint\n
					  # Si vous utilisez ce code merci de laisser ces remarques\n
				
	  #############################################################################
################\n
					  --&gt; \n\n\n\n\n &quot;;
					  
						include ('F
ICHIER DE CONNEXION DB');
						$produit = 'SELECT * FROM `produits` WHERE vent
e = 1 ORDER BY cat, id';
						$liste = mysql_query( $produit,$connexion );


						$trans = array ( &quot;è&quot; =&gt; &quot;e&quot;, &quot;é&quot; =&gt; &q
uot;e&quot;, &quot;à&quot; =&gt; &quot;a&quot;, &quot;ê&quot; =&gt; &quot;e&quot
;, &quot;½&quot; =&gt; &quot;1/2&quot;, &quot;ô&quot; =&gt; &quot;o&quot;, &quot
;ç&quot; =&gt; &quot;c&quot;, &quot;È&quot; =&gt; &quot;E&quot;, &quot;É&quot; =
&gt; &quot;E&quot;, &quot;À&quot; =&gt; &quot;A&quot;, &quot;Ê&quot; =&gt; &quot
;E&quot;, &quot;Ô&quot; =&gt; &quot;O&quot;, &quot;Ç&quot; =&gt; &quot;C&quot;);
 //suppression des accents pour formulaire transmit a PayPal car probleme avec c
odage des caracteres
						$tota1 = array(0);
						$frais1 = array(0);
					
	$prod1 = array(0);
						$prix1 = array(0);
						$quant = array(0);
						$
mail = array(0);
						$numprod = 0;
						$i = 0;
						if (isset($_POST[$va
r])) $_POST[$var]=&quot;0&quot;;
						while ($tablo = mysql_fetch_array($liste
)) 
						{
							if (!isset($_POST[$var])) $_POST[$var]=&quot;0&quot;;
				
			$i++; //Incrementee a chaques tour de boucle WHILE
							$var = 'var'.$i; /
/Construction des variables POST du formulaire

							if ( $_POST[&quot;reset
&quot;] ==&quot;on&quot; ) // reset des variables formulaires
							{
							
  $_POST[$var]= &quot;0&quot;;
							  $_POST['nom']= &quot;0&quot;;
							 
 $_POST['prenom']= &quot;0&quot;;
							  $_POST['message']= &quot;0&quot;;
	
						  $_POST['email']= &quot;0&quot;;
							  $_POST[&quot;envoi&quot;] =&qu
ot;&quot;;
							} //Vide tous les champs du formulaire

							if ($_POST[$
var]&gt; &quot;0&quot;) // si nombre d'articles commande superieurs a 0 pour une
 ligne
							{ 
							  $numprod++; // compte le nombre d'articles selection
nes
							} // Creation de la variable de generation item pour les variables p
roduits du bouton PayPal

							$prod  = $tablo['designation']; // designatio
n des articles
							$prodnohtm = strip_tags($prod); // supression des balises
 html dans designation des articles
							$proddec = html_entity_decode($prodn
ohtm, null, 'UTF-8'); // remplacement des entites html par le caractere accentue

							$prodno = strtr($prodnohtm, $trans); // remplacement des caracteres acc
entues par le caractere normal le plus proche ( supression des accents) pour for
mulaire transmit a PayPal							
							$prix = $tablo['prix']; // designation 
des articles

							if ($_POST[$var]&gt; &quot;0&quot;) // nombre d'articles 
commande superieurs a 0 pour une ligne 
							{ 
							  $prod1[$numprod] = 
$prodno; //inscrit tous les produits selectionnes dans une variable tableau
			
				  $quant[$numprod] = $_POST[$var]; //inscrit toutes les quantites produits s
electionnes dans une variable tableau
							  $prix1[$numprod] = $prix; //insc
rit tout les prix produits selectionnes dans une variable tableau
							}

	
						$trait = $tablo['traitement']; //variable frais de livraison-traitement ou
i ou non
							
							if ($prix&lt;&gt;0) //verifie qu'un prix ne soit pas e
gal a 0
							{
							  $prixht = ($prix/1.196)*$_POST[$var]; //calcule le p
rix ht a partir du ttc
							}

							$tva = round(($prixht*0.196),2); //ca
lcule le montant de la TVA
							$type = $tablo['type']; //affectation du prod
uit creation web ou depannage

							$total1[$i] = $prix*$_POST[$var]; //insc
rit le prix total de chaque ligne dans une variable tableau

							if ($trait
==1 &amp;&amp; $prix*$_POST[$var]&lt;&gt;0) //si frais de livraison-traitement o
ui et nombre de produits x par prix &gt;0
							{
							  $fraisunit = ((($p
rix*$_POST[$var])/100)*3.4)+0.25; //calcule les frais de traitement
							}
	
						else {$fraisunit = 0;} //si pas de frais de traitement la variable est ega
le a 0

							$frais1[$i] = $fraisunit; //inscrit les frais de traitement de 
chaque ligne dans une variable tableau

							if ( $trait == 1) //si frais de
 traitement oui
							{
							  $note = &quot;+ frais de traitement&quot;; /
/texte affiche pour frais de traitement oui
							} 
							else { $note = &q
uot;pas de frais de traitement&quot;;} //texte affiche pour frais de traitement 
non

							if ($_POST[$var] &gt; 0) // si nombre de produits &gt;0
							{ 

							  $imp = &quot;&quot;; // autorisation d'impression de la ligne
						
	}
							else { $imp = &quot;class='masimp' &quot;;} // interdiction d'impress
ion de la ligne

							if ( $_POST[&quot;reset&quot;] ==&quot;on&quot; ) // s
i demande remise a 0 du formulaire
							{
							  $chk = 'checked=&quot;che
cked&quot;'; // je force la case afficher les produits choisis a cochee
							
  $chk1 = 'checked=&quot;checked&quot;'; // je force la case afficher les produi
ts non choisis a cochee
							  $vu = &quot;&quot;; //je ne cache aucune ligne

							}
							else //pas de demande de remise a 0 du formulaire
							{

								if ($_POST[$var] &gt; 0 AND $_POST[&quot;masq&quot;] !=&quot;on&quot; AN
D $_POST[&quot;envoi&quot;] !=&quot;on&quot; ) //si nombre de produit &gt; 0 ET 
case afficher les produits choisis pas cochee ET case envoyer demande de devis c
ochee
								{
								  $vu = 'style=&quot;display : none;&quot;'; //je n'af
fiche pas la ligne
								}
								
								if ($_POST[$var] &gt; 0 AND $_P
OST[&quot;masq&quot;] ==&quot;on&quot; ) //si nombre de produit &gt; 0 ET case a
fficher les produits choisis cochee
								{
								  $vu = &quot;&quot;; //
j'affiche la ligne
								}

								if ($_POST[$var] == 0 AND ($_POST[&quot
;masq1&quot;] !=&quot;on&quot; OR $_POST[&quot;envoi&quot;] ==&quot;on&quot;) ) 
//si nombre de produit = 0 ET case afficher les produits non choisis pas cochee 
ou case envoyer demande de devis cochee
								{
								  $vu = 'style=&quot
;display : none;&quot;'; //je n'affiche pas la ligne
								}

								if ($
_POST[$var] == 0 AND ($_POST[&quot;masq1&quot;] ==&quot;on&quot;  AND $_POST[&qu
ot;envoi&quot;] !=&quot;on&quot;)) //si nombre de produit = 0 ET case afficher l
es produits non choisis cochee et case envoyer demande de devis pas cochee
				
				{
								  $vu = &quot;&quot;; //j'affiche la ligne
								}

								
 if ($_POST[&quot;masq1&quot;] !=&quot;on&quot; AND $_POST[&quot;masq&quot;] !=&
quot;on&quot; ) //si les cases afficher produits choisis et non choisis sont coc
hees
								{
								  $vu = &quot;&quot;; //j'affiche la ligne
								}


								if ( $_POST[&quot;masq&quot;] ==&quot;on&quot; OR $_POST[&quot;envoi&q
uot;] ==&quot;on&quot; ) //si case afficher les produits choisis cochee OU case 
envoyer demande de devis cochee
								{
								  $chk = 'checked=&quot;chec
ked&quot;'; //valeur par defaut de la case afficher les produits choisis a coche
e
								}
								else { $chk = &quot;&quot;;} //sinon valeur par defaut de 
la case afficher les produits choisis a pas cochee
								
								if ( $_POS
T[&quot;masq1&quot;] ==&quot;on&quot; AND $_POST[&quot;envoi&quot;] !=&quot;on&q
uot; ) //si case afficher les produits non choisis cochee ET case envoyer demand
e de devis pas cochee
								{
								  $chk1 = 'checked=&quot;checked&quot;
'; //valeur par defaut de la case afficher les produits non choisis a cochee
		
						}
								else { $chk1 = &quot;&quot;;} //sinon valeur par defaut de la c
ase afficher les produits non choisis a pas cochee
							}

							$code = &
quot;				&lt;tr &quot;.$imp.$vu.&quot;&gt;\n&quot;; // generation du code du tab
leau de produits
							    $code .=&quot;					&lt;td class='c1'&gt;\n&quot;;

								$code .=&quot;					  &lt;label&gt;&amp;nbsp;&quot;.$prod.&quot;: &lt;spa
n class='def' title='&quot;.round(($prix/1.196),2).&quot;€HT'&gt;&quot;.$prix.&q
uot;€&lt;sup&gt;TTC&lt;/sup&gt;&lt;/span&gt;&lt;/label&gt;\n&quot;;
								$co
de .=&quot;					&lt;/td&gt;\n&quot;;
								$code .=&quot;					&lt;td class='c
2'&gt;\n&quot;;
								$code .=&quot;					  &lt;input style='text-align : righ
t;' type='text' name='&quot;.$var.&quot;' size='3' value='&quot;.$_POST[$var].&q
uot;'&gt;\n&quot;;
								$code .=&quot;					&lt;/td&gt;\n&quot;;
								$co
de .=&quot;					&lt;td class='c2' &gt;\n&quot;;
								$code .=&quot;					  &q
uot;.round(($prixht),2).&quot;€&lt;sup&gt;HT&lt;/sup&gt; + &quot;.$tva.&quot;€ d
e TVA\n&quot;;
								$code .=&quot;					  &lt;br&gt;&quot;.$note.&quot;\n&quo
t;;
								$code .=&quot;					&lt;/td&gt;\n&quot;;
								$code .=&quot;				
&lt;/tr&gt;\n&quot;;

							if ($_POST[$var]&gt; &quot;0&quot;)
							{
		
					  $mail[$numprod] = $code; // inscription du code du tableau de produit dan
s une variable tableau
							}

							echo $code; // affichage  du code du 
tableau de produits
						}

						$total = array_sum($total1); // Je fais le
 total des totaux de chaque ligne
						$totalht = round(($total/1.196),2); // 
j'extrait le total global HT et j'arrondi a 2 decimales
						$frais = round(ar
ray_sum ($frais1),2); // Je fais le total des frais de chaque ligne et j'arrondi
 a 2 decimales
						$totaltva = round(($total - $totalht),2); // j'extrais la 
TVA et j'arrondi a 2 decimales
						$facture = round(($total + $frais),2); // 
je calcule le total de la facture et j'arrondi a 2 decimales

						if ($total
 == 0 ) // si la facture est d'un montant egal a 0 euro
						{ 
						  $chk 
= 'checked=&quot;checked&quot;'; //valeur par defaut de la case afficher les pro
duits choisis a cochee
						  $chk1 = 'checked=&quot;checked&quot;'; //valeur 
par defaut de la case afficher les produits choisis a cochee 
						  $env = &q
uot;style='display : none;'&quot;; // j'interdit l'affichage des options liees a
 un montant
						}
						else { $env = &quot;&quot;; } // sinon je n'interdit
 pas l'affichage des options liees a un montant

						$code1 = &quot;				&lt;
tr&gt;\n&quot;; // generation du code des tataux en bas de tableau
							$code
1 .= &quot;					&lt;td class='c1' colspan='3'&gt;\n&quot;;
							$code1 .= &qu
ot;					  &lt;br&gt;Total &quot;.$totalht.&quot;€&lt;sup&gt;HT&lt;/sup&gt;\n&quo
t;;
							$code1 .= &quot;					  &lt;br&gt;&quot;.$totaltva.&quot;€ de TVA\n&q
uot;;
							$code1 .= &quot;					  &lt;br&gt;&quot;.$frais.&quot;€ de frais de
 traitement\n&quot;;
							$code1 .= &quot;					  &lt;br&gt;Soit une facture t
otale de :&quot;.$facture.&quot;€&lt;sup&gt;TTC&lt;/sup&gt;\n&quot;;
							$co
de1 .= &quot;					&lt;/td&gt;\n&quot;;
							$code1 .= &quot;				&lt;/tr&gt;\n
&quot;;
							$code1 .= &quot;			&lt;/table&gt;\n&quot;;

						echo $code1;
  // affichage du code des tataux en bas de tableau

						$headers ='From: &q
uot;Devis&quot;&lt;VOTRE@EMAIL&gt;'.&quot;\n&quot;; // entete email devis
					
		$headers .='Content-Type: text/html; charset=&quot;utf-8&quot;'.&quot;\n&quot;
;
							$headers .='Content-Transfer-Encoding: 8bit';

						$mailtot = &quo
t;&lt;h1&gt;Vous avez reçu une demande de devis !&lt;/h1&gt;\n &lt;p&gt;&quot;; 
// generation du code du mail de devis
							$mailtot .= $_POST[&quot;message&
quot;].&quot;\n &lt;br&gt; de: &quot;.$_POST[&quot;prenom&quot;].&quot;&amp;nbsp
;&quot;.$_POST[&quot;nom&quot;];
							$mailtot .= &quot;&lt;br&gt;&quot;.$_PO
ST[&quot;email&quot;].&quot;&lt;/p&gt;\n&lt;table&gt;&quot;.implode($mail).$code
1; // implode de la variable tableau contenant le code du tableau de produits


						if ( $_POST[&quot;envoi&quot;] ==&quot;on&quot; ) // si demande d'envoi d
e devis par mail
						{
						  $chk2 = 'checked=&quot;checked&quot;';	//vale
ur par defaut de la case afficher les produits choisis a cochee
						  $chk = 
'checked=&quot;checked&quot;';	//valeur par defaut de la case afficher les produ
its choisis a cochee
						  $chk1 = '';					//valeur par defaut de la case aff
icher les produits non choisis a pas cochee
						  $action = 'Envoyer'; // tex
te du bouton de soumission

							if ($_POST[&quot;email&quot;]&lt;&gt; &quot
;&quot; &amp;&amp; eregi(&quot;^[_a-z0-9-]+(\.[_a-z0-9-]+)*@[a-z0-9-]+(\.[a-z0-9
-]+)*$&quot;,$_POST[&quot;email&quot;]) ) // verification presence et vatidite d
e la saisie de l'email du client
							{ 
							  mail('VOTRE@EMAIL','Demand
e de devis', $mailtot, $headers); //envoi du devis au gestionnaire du site
				
			  mail($_POST[&quot;email&quot;],'Copie du devis soumis', $mailtot, $headers)
; //envoi du devis au client
							  $contfor = &quot;&lt;div style='display :
 none;'&gt;&quot;; //masque formulaire et autre si envoi OK
							  $contfor1 
= &quot;&lt;/div&gt;&quot;;	//masque formulaire et autre si envoi OK
							  $
_POST[&quot;envoi&quot;] = &quot;&quot;; //intitialise le formulaire de contact

							  $_POST[&quot;nom&quot;] = &quot;&quot;;
							  $_POST[&quot;prenom&
quot;] = &quot;&quot;;
							  $_POST[&quot;email&quot;] = &quot;&quot;;
				
			  $_POST[&quot;message&quot;] = &quot;&quot;;
							  $chk2 = &quot;&quot;;

							  echo &quot;&lt;h2&gt;Votre demande à bien été envoyée&lt;/h2&gt;&quot
;;
							}
							else { echo &quot;&lt;h3 class='masimp' &gt;Vérifiez la sai
sie de votre adresse Email.&lt;/h3&gt;&quot;; }	//previent d'un probleme avec ad
resse email
							// formulaire de contact
							echo &quot;&lt;div class='m
asimp'&gt;\n&quot;.$contfor.&quot;\n&lt;label&gt;Nom:&amp;nbsp;&lt;input style='
text-align : left;' type='text' name='nom' size='25' value='&quot;.$_POST[&quot;
nom&quot;].&quot;' &gt;&lt;/label&gt;\n&quot;;
							echo &quot;&lt;br&gt;&lt;
br&gt;&lt;label&gt;Prénom:&amp;nbsp;&lt;input style='text-align : left;' type='t
ext' name='prenom' size='25' value='&quot;.$_POST[&quot;prenom&quot;].&quot;' &g
t;&lt;/label&gt;\n&quot;;
							echo &quot;&lt;br&gt;&lt;br&gt;&lt;label&gt;Em
ail:&amp;nbsp;&lt;input style='text-align : left;' type='text' name='email' size
='25' value='&quot;.$_POST[&quot;email&quot;].&quot;' &gt;&lt;/label&gt;\n&quot;
;
							echo &quot;&lt;br&gt;&lt;br&gt;&lt;label&gt;Message:&amp;nbsp;&lt;text
area style='text-align : left;' name='message' cols='25' rows='15'&gt;&quot;.$_P
OST[&quot;message&quot;].&quot;&lt;/textarea&gt;&lt;/label&gt;\n&quot;.$contfor1
.&quot;\n&lt;/div&gt;\n&quot;;
						}
						else { $chk2 = ''; $action = 'Cal
culer';}	// texte du bouton de soumission
						// diverses options d'affichage

 						echo &quot;			&lt;div class='masimp' style='width : 28em; margin-left :
 auto; margin-right : auto; text-align : left;' &gt;\n\n&quot;;
						echo &quo
t;     	 	  &lt;span &quot;.$env.&quot;&gt;&lt;input type='checkbox' name='envoi
' &quot;.$chk2.&quot; class='button'&gt;&amp;nbsp;Cochez cette case puis cliquez
 sur&amp;nbsp;&lt;/span&gt;\n&quot;;
						echo &quot;			  &lt;input type='subm
it' name='action' value='&quot;.$action.&quot;'&gt;\n&quot;;
						echo &quot;	
			&lt;span &quot;.$env.&quot;&gt;\n&quot;;
						echo &quot;				  &lt;br&gt;po
ur demander un devis définitif\n&quot;;
						echo &quot;				  &lt;br&gt;\n&quo
t;;
						echo &quot;				  &lt;br&gt;\n&quot;;
						echo &quot;					&lt;scrip
t type='text/JavaScript'&gt;\n&quot;;
						echo &quot;					  document.write('&
lt;button onClick=\'window.print();\'\&gt;Imprimer le devis&lt;\/button\&gt;');\
n&quot;;
						echo &quot;					&lt;/script&gt;\n&quot;;
						echo &quot;				 
 &lt;br&gt;\n&quot;;
						echo &quot;				&lt;/span&gt;\n\n&quot;;
						echo 
&quot;				  &lt;br&gt;\n&quot;;
						echo &quot;				  &lt;input type='checkbox
' name='reset' class='button'&gt;Cochez pour vider votre panier.\n&quot;;
					
	echo &quot;				  &lt;br&gt;\n&quot;;
						echo &quot;				  &lt;input type='ch
eckbox' name='masq' &quot;.$chk.&quot; class='button'&gt;cochez pour voir les pr
oduits que vous avez choisis.\n&quot;;
						echo &quot;				  &lt;br&gt;\n&quot
;;
						echo &quot;				  &lt;input type='checkbox' name='masq1' &quot;.$chk1.&
quot; class='button'&gt;Cochez pour voir les produits non choisis.\n\n&quot;;
	
					echo &quot;			&lt;/div&gt;\n&quot;;
						echo &quot;		 &lt;/form&gt;\n&qu
ot;;
					?&gt;

			&lt;div class=&quot;masimp&quot;&lt;?php echo $env; ?&gt;
&gt;
			  &lt;br&gt;
			  &lt;br&gt;
			  &lt;script type=&quot;text/JavaScri
pt&quot;&gt;
			  document.write('&lt;div style=\&quot;float : right;\&quot;\&g
t;En cliquant sur ce bouton vous lancez une impression de vos choix&lt;br\&gt;ET
 accédez directement à la page de paiement.&lt;\/div\&gt;');
			  &lt;/script&g
t;
			  &lt;noscript&gt;&lt;div style=&quot;float : right;&quot;&gt;En cliquant
 sur ce bouton vous accédez directement à la page de paiement.&lt;/div&gt;&lt;/n
oscript&gt;
				&lt;div style=&quot;float : right;&quot; onClick=&quot;window.p
rint();&quot; &gt;
					&lt;form target=&quot;paypal&quot; action=&quot;<a href
='https://www.paypal.com/cgi-bin/webscr' target='_blank'>https://www.paypal.com/
cgi-bin/webscr</a>&quot; method=&quot;post&quot;&gt;
					  &lt;input type=&quo
t;image&quot; src=&quot;URL ABSOLUE DE L'IMAGE DU BOUTON PAYPAL&quot; name=&quot
;submit&quot; alt=&quot;PayPal - la solution de paiement en ligne la plus simple
 et la plus sécurisée !&quot;&gt;
					  &lt;img alt=&quot;&quot; border=&quot;
0&quot; src=&quot;<a href='https://www.paypal.com/fr_FR/i/scr/pixel.gif' target=
'_blank'>https://www.paypal.com/fr_FR/i/scr/pixel.gif</a>&quot; width=&quot;1&qu
ot; height=&quot;1&quot;&gt;
					  &lt;input type=&quot;hidden&quot; name=&quo
t;upload&quot; value=&quot;1&quot;&gt;
					  &lt;input type=&quot;hidden&quot;
 name=&quot;cmd&quot; value=&quot;_cart&quot;&gt;
					  &lt;input type=&quot;h
idden&quot; name=&quot;business&quot; value=&quot;VOTRE CODE DE PARAINAGE OU VOT
RE EMAIL IDENTIFIANT PAYPAL&quot;&gt;
						&lt;?php
							for($bouc=1; $bouc
&lt;=$numprod; $bouc++) // creation d'une boucle reiteree autant de fois q'un pr
oduit a ete choisi
							{
							  echo '&lt;input type=&quot;hidden&quot; n
ame=&quot;item_name_'.($bouc).'&quot; value=&quot;'.$prod1[$bouc].'&quot;&gt;'.&
quot;\n&quot;; // transmition de la designation des produits
							  echo '&lt
;input type=&quot;hidden&quot; name=&quot;quantity_'.($bouc).'&quot; value=&quot
;'.$quant[$bouc].'&quot;&gt;'.&quot;\n&quot;;  // transmition de la quantite des
 produits
							  echo '&lt;input type=&quot;hidden&quot; name=&quot;amount_'.
($bouc).'&quot; value=&quot;'.$prix1[$bouc].'&quot;&gt;'.&quot;\n&quot;;	// tran
smition du prix des produits
							}
						?&gt;

					  &lt;input type=&qu
ot;hidden&quot; name=&quot;handling_1&quot; value=&quot;&lt;?php echo $frais; //
 transmition des frais de traitement livraison globaux ?&gt;&quot;&gt;
					  &
lt;input type=&quot;hidden&quot; name=&quot;logo_custom&quot; value=&quot;URL AB
SOLUE DE L'IMAGE DU BOUTON PAYPAL&quot;&gt;
					  &lt;input type=&quot;hidden&
quot; name=&quot;no_note&quot; value=&quot;1&quot;&gt;
					  &lt;input type=&q
uot;hidden&quot; name=&quot;currency_code&quot; value=&quot;EUR&quot;&gt;
					
  &lt;input type=&quot;hidden&quot; name=&quot;lc&quot; value=&quot;FR&quot;&gt;

					  &lt;input type=&quot;hidden&quot; name=&quot;bn&quot; value=&quot;PP-Sh
opCartBF&quot;&gt;
					&lt;/form&gt;
				&lt;/div&gt;
			&lt;/div&gt;
</pre
>
<br /><a name='conclusion'></a><h2> Conclusion : </h2>
<br />Bon, c'est mon
 premier code ici, &ccedil;a fait un mois que je m'essaie au PHP, et quelques an
n&eacute;es que je pique sournoisement des bouts de codes ici.
<br />Alors si c
e petit truc maladroit pouvait &ecirc;tre utile &agrave; quelqu'un ce serait un 
juste retour.
<br />Comme je l'ai dit je suis Newbie alors les critiques seront
 les bien venues.
