# Spotify API Postman Collection

This repository contains a Postman collection for interacting with the Spotify API. The collection includes various requests to manage albums, playlists, and more.

## Getting Started

### Prerequisites

- **Postman**: Make sure you have [Postman](https://www.postman.com/downloads/) installed.
- **Spotify Developer Account**: You need a Spotify developer account to obtain your Client ID, Client Secret, and other necessary credentials. Sign up at the [Spotify Developer Dashboard](https://developer.spotify.com/dashboard/).

### Setting Up the Collection

1. **Clone the Repository**:
    ```sh
    git clone https://github.com/your-username/spotify-api-postman-collection.git
    cd spotify-api-postman-collection
    ```

2. **Import the Collection into Postman**:
    - Open Postman.
    - Click on the **Import** button in the top-left corner.
    - Select the `spotify_api_collection.json` file from this repository.

3. **Configure Environment Variables**:
    - In Postman, go to **Environments** and create a new environment named `Spotify API`.
    - Add the following variables to your environment:
        - `client_id`: Your Spotify Client ID.
        - `client_secret`: Your Spotify Client Secret.
        - `refresh_token`: Your Spotify Refresh Token.
        - `access_token`: Your Spotify Access Token (this can be managed by a pre-request script for automatic token refresh).

### OAuth 2.0 Authentication

This collection uses OAuth 2.0 for authentication. Ensure your environment is set up with the necessary tokens.

#### Pre-request Script for Token Refresh

To automate the token refresh process, you can add the following script to the **Pre-request Script** tab of your collection or requests:

```javascript
pm.sendRequest({
    url: 'https://accounts.spotify.com/api/token',
    method: 'POST',
    header: {
        'Content-Type': 'application/x-www-form-urlencoded'
    },
    body: {
        mode: 'urlencoded',
        urlencoded: [
            { key: 'grant_type', value: 'refresh_token' },
            { key: 'refresh_token', value: pm.environment.get('refresh_token') },
            { key: 'client_id', value: pm.environment.get('client_id') },
            { key: 'client_secret', value: pm.environment.get('client_secret') }
        ]
    }
}, function (err, res) {
    if (err) {
        console.log('Error getting new access token:', err);
    } else {
        var jsonData = res.json();
        pm.environment.set('access_token', jsonData.access_token);
    }
});
