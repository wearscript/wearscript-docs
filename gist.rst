Gist Integration
=================

.. raw:: html

  <div style="position: relative;margin-bottom: 30px;padding-bottom: 56.25%;padding-top: 30px; height: 0; overflow: hidden;">
    <iframe style="position: absolute;top: 0;left: 0;width: 100%;height: 100%;" src="http://www.youtube.com/embed/LEXDaZzvSKk" frameborder="0"></iframe>
  </div>

WearScript uses Github's Gist service to facilitate storing and sharing scripts.  The Go server (wearscript-server) communicates with the Gist API and only allows operation on gists that have [wearscript] as the prefix of the description.  The default script name for Glass devices is glass.html and there should be a manifest.json file with a field "name" that is the script's name.  Gists can be synced to Glass by using the Gist Sync option in the menu when you start WearScript.  In the playground gists can be opened, forked from others, and modified.
