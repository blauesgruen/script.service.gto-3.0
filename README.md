<h1>Description:</h1>

The script 'script.service.gto' fetches actual tv highlights from various RSS feeds and present them as widget in the home 
menu (needs skin integration). It can show only those events optionally if they are present in the pvr channel list.

The script starts as a service which runs at Kodi start and fetches the data depending on the selected interval. In addition 
to update all outdated broadcasts will removed from the widget list.

## Skintegration:

To integrate the TV highlights of the day in Estuary, please follow the instructions in the README.md  in the folders `integration/<skinname>`. Otherwise, 
the addon can be integrated into other skins as a TV Widget too. The addon use a method called "Dynamic List Content". 
The integration by the skinner is done by calling:

    <content target="pvr">plugin://script.service.gto?action=getcontent&amp;ts=$INFO[Window(Home).Property(GTO.timestamp)]</content>

For detailed description of ListItem.Labels and ListItem.Properties see below.

## Script call (Methods):

The service for updating content and widgets (starter.py) calls the actual plugin via the parameter 'action' with a 
corresponding value. This parameter can be used by other plugins or scripts as follows:

#### Perform a rescan of the feeds and refresh the widget:

    RunPlugin(plugin://script.service.gto?action=scrape)

#### Refresh the Widget and remove outdated items:

    RunPlugin(plugin://script.service.gto?action=getcontent)

#### Opens a scraper list and stores the selected scraper as default:

    RunPlugin(plugin://script.service.gto?action=change_scraper)
    
#### Opens a window with additional details of the selected broadcast - Important for Skinners!:

Example 'onclick' of a selected element - opens a popup window (<skin.id>.script-gto-info.xml):

    <onclick>
        RunPlugin(plugin://script.service.gto?action=info&amp;item=$INFO[ListItem.Property(Item)])
    </onclick>
    
#### Set a record timer within from info window

    <onclick>
        RunPlugin(plugin://script.service.gto?action=record&amp;broadcastid=$INFO[Window(Home).Property(GTO.Info.BroadcastID)]&amp;item=$INFO[Window(Home).Property(GTO.Info.Item)])
    </onclick>
    
#### List items, Info labels and Properties:

    - ListItem.Label                  Station name and start time (hh:mm)
    - ListItem.Label2                 Title of broadcast
    - ListItem.Art(icon)              Screenshot of broadcast
    - ListItem.Art(thumb)             dto.
    - ListItem.Art(poster)            dto.
    - ListItem.Art(fanart)            dto.

    - ListItem.Art(logo)              PVR station logo
    
    - ListItem.genre                  Genre
    - ListItem.plot                   Content description of broadcast
    - ListItem.duration               Runtime in minutes
    - ListItem.rating                 Rating
    
    - ListItem.Property(StartTime)    Datetime of broadcast start, regional settings without secs
    - ListItem.Property(EndTime)      End time
    - ListItem.Property(RunTime)      Runtime of broadcast in minutes
    - ListItem.Property(Item)         Item number

#### Properties for Home window (resides as properties in Window(Home)):

    - GTO.provider                    Provider/Anbieter (scraper.friendlyname)
    - GTO.items                       count of items
    - GTO.busy                        true/false - scraper is busy
    - GTO.timestamp                   Timestamp of last refresh of the widget

#### Properties for info window (resides as properties in Window(Home)):

    - GTO.Info.Item                   ID of item
    - GTO.Info.Title                  title
    - GTO.Info.Picture                screenshot of broadcast
    - GTO.Info.Description            description
    - GTO.Info.Channel                PVR channel name
    - GTO.Info.ChannelID              PVR channel id
    - GTO.Info.Logo                   PVR channel logo
    - GTO.Info.Date                   datetime in format 'dd.mm.yy hh:mm'
    - GTO.Info.RunTime                run time in minutes
    - GTO.Info.EndTime                end time (hh:mm)
    - GTO.Info.Rating                 Genre and/or Rating
    - GTO.Info.hasTimer               'True|False' active recording timer
    - GTO.Info.BroadcastID            Broadcast-ID of timer
    - GTO.Info.isInFuture             'True|False' broadcast is in future
    - GTO.Info.isRunning              'True|False' broadcast is currently running

### FAQ (German only)

Q: Hinter den Sendernamen sind Sternchen (*), warum ist das so?

   Der ausgewiesene Sendername des Scrapers ist ein anderer als der aus der Senderliste (Bsp: RTLII <> RTL2). In diesem 
   Fall musst die 'ChannelTranslate.json' angepasst werden, die diverse vom Scraper verwendete Sendernamen auf die PVR-Liste mappt. 
   Damit die Transformationstabelle nicht bei jedem Update überschrieben wird, liegt diese unter

        Linux (Kodi, LibreElec): /storage/.kodi/userdata/addon_data/script.service.gto/translations.json)
        Windows: %APPDATA%\kodi\userdata\addon_data\script.service.gto\translations.json
    
   Die 'translations.json' kann mit jedem einfachen Texteditor bearbeitet werden
