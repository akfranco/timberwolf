# timberwolf
## A private torrent tracker based on [bittorrent-tracker](https://github.com/feross/bittorrent-tracker) written in [Node.js](http://github.com/joyent/node)

#### Note: Only use this software willing to extensively test its stability for your applications or willing to not care too much about the consequences of _NOT_ testing it.

This project is optimized for use as a private tracker. All requests flow through a user authentication function first, and then must subsequently pass torrent client whitelisting checks followed by the existence of the torrent itself on the tracker. Stats are cached locally and then written to a persistent database at a set time out.

## General Architecture
```mermaid
graph TD
    A["GET *"] --> B["User Authentication"]
    B -- "Fail" --> D["Return error"]
    B -- "Pass" --> C["Router"]
    
    C -- "Pass" --> E["/flush"]
    C -- "Pass" --> F["/announce"]
    C -- "Pass" --> G["/scrape"]
    C -- "Fail" --> D
    
    F --> H["Is client whitelisted?"]
    
    H -- "Fail" --> D
    H -- "Pass" --> I["Does torrent exist?"]
    
    I -- "Fail" --> D
    I -- "Pass" --> J["Save user params"]
    
    J --> K["Return swarm"]
    
    G --> D

    %% Styling to match the original image colors
    classDef green fill:#b5e5b8,stroke:#90b893,stroke-width:1px,color:#000
    classDef red fill:#f69c9b,stroke:#c47c7c,stroke-width:1px,color:#000
    classDef yellow fill:#ffe495,stroke:#cca85e,stroke-width:1px,color:#000
    classDef blue fill:#9fb7e0,stroke:#7c92b8,stroke-width:1px,color:#000

    class A,B,C,F,G,J,K green
    class D red
    class E yellow
    class H,I blue```

## Usage
```git clone [.git path]```

Clone project into your working directory. Using default.json as your template, create /config/production.json to house your server settings. Then

```npm start```

or

```node app.js```

and it should begin tracking as expected.

### How To Contribute
Fork this project and create a pull request.

Enjoy!