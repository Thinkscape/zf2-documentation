.. _zend.log.writers.zendmonitor:

Ecrire vers le moniteur Zend Server
===================================

``Zend_Log_Writer_ZendMonitor`` vous permet de journaliser des évènements via l'API de Zend Server. Vous pouvez
alors aggréger des messages de journal pour l'application et tout son environnement, ceci vers un seul endroit. En
interne, cet objet utilise simplement la fonction ``monitor_custom_event()`` issue de Zend Monitor.

Une caractéristique particulière de l'API Monitor est que vous pouvez spécifier n'importe quelle information
dans le journal. Par exemple, journaliser une exception est possible en journalisant tout l'objet Exception d'un
coup et pas juste son message. L'objet sera alors visible et analysable via le moniteur d'évènement de Zend
Server.

.. note::

   **Zend Monitor doit être installé et activé**

   Pour utiliser cet objet d'écriture, Zend Monitor doit petre installé et activé. Si ce n'est pas le cas, alors
   l'objet d'écriture agira de manière transparente et ne fera rien du tout.

Instancier l'objet d'écriture ``ZendMonitor`` est très simple:

.. code-block:: php
   :linenos:

   $writer = new Zend_Log_Writer_ZendMonitor();
   $log    = new Zend_Log($writer);

Ensuite, journalisez vos évènements comme d'habitude:

.. code-block:: php
   :linenos:

   $log->info('Voici un message');

Vous pouvez ajouter des informations à journaliser, passez les comme second paramètre:

.. code-block:: php
   :linenos:

   $log->info('Exception rencontrée', $e);

Ce deuxième paramètre peut être de type scalaire, objet, ou tableau; si vous souhaitez passer plusieurs
informations d'un seul coup, utilisez un tableau.

.. code-block:: php
   :linenos:

   $log->info('Exception rencontrée', array(
       'request'   => $request,
       'exception' => $e,
   ));

Au sein de Zend Server, votre évènement est enregistré comme un "évènement personnalisé" (custom event).
Depuis l'onglet "Monitor", sélectionnez le sous menu "Evènements"(Events), et utilisez ensuite le filtre
"Personnalisé"(Custom).

.. image:: ../images/zend.log.writers.zendmonitor-events.png


Evènements dans le récapitulatif Zend Server Monitor

Sur cette image, les deux premiers évènements listés sont des évènements personnalisés enregistré via
l'objet d'écriture ``ZendMonitor``. Cliquez alors sur un évènement pour voir toutes ses informations.

.. image:: ../images/zend.log.writers.zendmonitor-event.png


Détails de l'évènement dans Zend Server Monitor

Cliquer sur le sous menu "Personnalisé"(Custom) montre les détails, c'est à dire ce que vous avez passé comme
deuxième argument à la méthode de journalisation. Cet information est enregistrée sous la clé ``info``; et
vous pouvez voir que l'objet de requête a été enregistré dans cet exemple.

.. note::

   **Intégration avec Zend_Application**

   Par défaut, les commandes ``zf.sh`` et ``zf.bat`` ajoute une configuration pour la ressource 'log' de
   :ref:`Zend_Application <zend.application.available-resources.log>`, elle inclut la configuration pour l'objet
   d'écriture du journal ``ZendMonitor``. Aussi, le ``ErrorController`` utilise ce journal pour journaliser les
   exceptions de l'application.

   Comme dit précedemment, si l'API de Zend Monitor API n'est pas détectée sur votre installation de PHP, alors
   le journal ne fera rien du tout.


