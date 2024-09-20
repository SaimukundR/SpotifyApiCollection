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
```

## Usage

### Albums

- **Get Album Details**: Fetch detailed information about a specific album.

     GET https://api.spotify.com/v1/albums/{album_id}

  
### Playlists

- **Create New Playlist**: Create a new playlist for a user.

     POST https://api.spotify.com/v1/users/{user_id}/playlists


- **Change Playlist Details**: Modify details of a playlist.

     PUT https://api.spotify.com/v1/playlists/{playlist_id}


- **Get Playlist**: Retrieve a playlist's details.

     GET https://api.spotify.com/v1/playlists/{playlist_id}


- **Add Items to Playlist**: Add tracks to a playlist.

     POST https://api.spotify.com/v1/playlists/{playlist_id}/tracks


- **Delete Items from Playlist**: Remove tracks from a playlist.

     DELETE https://api.spotify.com/v1/playlists/{playlist_id}/tracks


---

## Running the Collection with Newman

You can run this collection using **Newman**, Postman's command-line runner.

### Install Newman:

```sh
npm install -g newman
```
```sh
newman run spotify_api_collection.json -e spotify_api_environment.json
```

## Contributing

-*Feel free to open issues or submit pull requests if you find any bugs or want to add new features.*

## License

-*This project is licensed under the MIT License.*

