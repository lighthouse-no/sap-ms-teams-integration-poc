# Configure the Bot to use the Log Analytics Workspace Instance

Select your bot from *Home* &rarr; *Resource Groups* &rarr; &lt;your resource group&gt; &rarr; &lt;bot name&gt;, then under *Monitoring*, select *Diagnostic settings*.

![Add Diagnostics](../img/azure_bot_add_diagnostics.png)

Give the diagnostic configuration a name (in our case `lh_convo_bot_diagnostics`), then select:

* Requests from the channels to the bot
* All metrics
* Send these massages to the log analytics workspace we've just created

![Configure Bot Diagnostics](../img/azure_bot_diagnostics_config.png)
