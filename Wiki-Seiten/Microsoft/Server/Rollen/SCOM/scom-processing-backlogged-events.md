# SCOM Processing Backlogged Events

Um diese Fehlermeldung zu lösen gibt es zwei Methoden

<div class="vector-body" id="bkmrk-from-the-console.-sc"><div class="mw-body-content mw-content-ltr" dir="ltr" id="bkmrk-from-the-console.-sc-1" lang="de"><div class="mw-parser-output">1. From the console. Scope yourself down to te Agent Health State. Select the "Flush Health Service State and Cache" task form the Actions menu.
2. Manually. Stop the health service on the affected agent. Delete the contents of the folder "Program Files\\System Center Operations Manager 2007\\Health Service State\\Health Service Store". Restart the health service.

</div></div></div>