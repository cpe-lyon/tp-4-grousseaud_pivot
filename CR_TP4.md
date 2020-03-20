
# TP 4 administration système 

## Exercice 1:

1) Commencez par créer deux groupes groupe1 et groupe2
>On utilise add group suivit du nom du groupe à créer. 

2) Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec la commande useradd, en demandant la création de
leur dossier personnel et avec bash pour shell
>sudo user add nom - m, le-m permet de créer un dossier personnel pour l'utilisateur basé dans /home

3) Ajoutez les utilisateurs dans les groupes créés :
- u1, u2, u4 dans groupe1
- u2, u3, u4 dans groupe2
>sudo usermod - a - g nom_groupe nom_user
Pour créer un groupe primaire, c'est le premier groupe auquel est associé l'utilisateur. Il peut cependant faire de plusieurs.

4) Donnez deux moyens d’afficher les membres de groupe2
>cat /etc/group |grep nom_groupe
Ou 
grep - w nom_groupe /etc/groupe

5) Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire
de /home/u3 et /home/u4
>sudo usermod -m -d groupe1 u1
sudo usermod -m -d groupe1 u2
sudo usermod -m -d groupe2 u3
sudo usermod -m -d groupe2 u4


6) Remplacez le groupe primaire des utilisateurs :
- groupe1 pour u1 et u2
- groupe2 pour u3 et u4
>sudo usermod nom_user - g nom_groupe
Permet de remplacer le groupe primaire des utilisateurs

7) Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et
mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier
associé.
>sudo chmod 170 nom_dossier, 170  autorise l'utilisateur à parcourir le dossier, 7 donne tout les droits aux membres du groupe et 0 ne donne aucun droit aux autres utilisateurs. 

8) Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer
ou supprimer ce fichier ?
>sudo chmod go-rwx
ou
sudo chmod a-rwx
sudo chmod u+rwx

9) Pouvez-vous vous connecter en tant que u1 ? Pourquoi ?
>on ne peut pas se cnnecter car on a pas définit de mdp, l'utilisateur est inactif jusqu'à la création d'un mdp. 

10) Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son
compte.
>sudo passwd nom_user
Puis on rentre le mot de passe voulu. 
On peut donc désormais se connecter en tant que u1( test=> su u1 pour se connecter)

11) Quels sont l’uid et le gid de u1 ?
>pour afficher les id d'utilisateur et de groupe on tape id nom_user:
Pouru1:
uid =1004 user ID 
gid=1001 groupe ID

12) Quel utilisateur a pour uid 1003 ?
>Il s'agit de u2

13) Quel est l’id du groupe groupe1 ?
>gid=1001

14) Quel groupe a pour guid 1002 ?
>gid=1002 correspond au groupe 2

15) Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez
>sudo gpasswd -d nom_user nom_groupe est la commande qui permet de retirer un utilisateur d'un groupe, on ne voit pas de changement. On doit sûrement mettre à jour les donnés du terminal.

16) Modifiez le compte de u4 de sorte que :
— il expire au 1
er juin 2020
— il faut changer de mot de passe avant 90 jours
— il faut attendre 5 jours pour modifier un mot de passe
— l’utilisateur est averti 14 jours avant l’expiration de son mot de passe
— le compte sera bloqué 30 jours après expiration du mot de passe
>

17) Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ?
>

18) à quoi correspond l’utilisateur nobody ?
>un compte user à qui aucun fichier n'appartient, qui n'appartient à aucun groupe, on l'utilise pour exécuter qlq chose sans aucune conséquence sur le reste. 
 
 19) Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ?
Quelle commande permet de forcer sudo à oublier votre mot de passe ?
>

## Exercice 2

1) Dans votre $HOME, créez un dossier test, et dans ce dossier un fichier fichier contenant quelques
lignes de texte. Quels sont les droits sur test et fichier ?

> sudo mkdir test
sudo nano fichier fichier
Pour regarder les droits: ls -l
droit sur fichier(ecrit) : -rw-r--r-- 1 root root
droit sur test :  drwxr-xr-x root root

2) Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’afficher en tant que root. Conclusion ?
>sudo chmod 000 fichier
En tant que root le fichier s’affiche normalement (sudo)
En tant qu’utilisateur, un ficheir vide s'ouvre.

3) Redonnez vous les droits en écriture et exécution sur fichier puis exécutez la commande echo "echo
Hello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’un
fichier s’il existe déjà. Que peut-on dire au sujet des droits ?
>sudo chmod 600 fichier
la commande est refusé car on n’a pas les droits d'écriture.

4) Essayez d’exécuter le fichier. Est-ce que cela fonctionne ? Et avec sudo ? Expliquez.
>On peut l’executer mais on ne voit pas le contenu.
Avec sudo on peut manipuler le fichier normalement.

5. Placez-vous dans le répertoire test, et retirez-vous le droit en lecture pour ce répertoire. Listez le
contenu du répertoire, puis exécutez ou affichez le contenu du fichier fichier. Qu’en déduisez-vous ?
Rétablissez le droit en lecture sur test
>Cela ne marche pas, il faut se mettre dans le repertoire précédent,pour executer : sudo chmod -R 333 test
On a enlever les droits de lecture, ainsi en executant la commande ls dans le repertoire test, on a le message d'erreur :  Permission non accordée.

6. Créez dans test un fichier nouveau ainsi qu’un répertoire sstest. Retirez au fichier nouveau et au
répertoire test le droit en écriture. Tentez de modifier le fichier nouveau. Rétablissez ensuite le droit
en écriture au répertoire test. Tentez de modifier le fichier nouveau, puis de le supprimer. Que pouvezvous déduire de toutes ces manipulations ?
>sudo chmod 500 sstest nouveau
Lorsque le repertoire parent a les droits d'ecriture alors il peut ecrire dans l'un de ses fichiers même si celui ci ne les a pas.

7. Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test.
Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’en
lister le contenu, etc…Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires ?
>On a enlevé les droits d'éxecution de test, comme c'est un dossier on ne peut pas rentrer dedans.
Cela ne sert a rien car on ne peut plus parcourir les dossiers, mais pour les proteger cela est pertinent.

8. Rétablissez le droit en exécution du répertoire test. Positionnez vous dans ce répertoire et retirez lui
à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire
test, de vous déplacer dans ssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence des
droits que l’on possède sur le répertoire courant ? Peut-on retourner dans le répertoire parent avec ”cd
..” ? Pouvez-vous donner une explication ?
>sudo chmod a-x * enleve les droits a tout le monde (a-g-o) sur les fichiers a l'interieur. Mais on n'arrive pas à modifier les droits du dossier courant.

9. Rétablissez le droit en exécution du répertoire test. Attribuez au fichier fichier les droits suffisants
pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.
>Même problème..

10. Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture,
ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.
>umask 077 les bits sont ici en non-et et donc le 0 autorise tout(rwx) et le 7 rien. 

11. Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos répertoires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.
> umask 022 (2 correspond a w=write).

12. Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture aux
membres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.
>umask 037

13. Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vous
pourrez vous aider de la commande stat pour valider vos réponses) :
- chmod u=rx,g=wx,o=r fic
- chmod uo+w,g-rx fic en sachant que les droits initiaux de fic sont r--r-x---
- chmod 653 fic en sachant que les droits initiaux de fic sont 711
- chmod u+x,g=w,o-r fic en sachant que les droits initiaux de fic sont r--r-x---
>-chmod 534 fic
-chmod 620 fic
-chmod u-x,g+r,o+w fic
-chmod 520 fic

14. Affichez les droits sur le programme passwd. Que remarquez-vous ? En affichant les droits du fichier
/etc/passwd, pouvez-vous justifier les permissions sur le programme passwd ?
>ls -l passwd
Tout le monde a le droit de lecture mais seul l'utilisateur a le droit d'écriture et personne n'a le droit d'execution.



