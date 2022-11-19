# dogechain-snapshots

## Database:

The current database is for latest [dogechain client](https://github.com/dogechain-lab/dogechain/releases/latest).

### Endpoint

[dogechain.2022-11-17T04_28_58-UTC.snapshot.zst](http://snapshots.dogechain.dog/dogechain.2022-11-17T04_28_58-UTC.snapshot.zst)

* *file size: 445GB*.
* *sha256sum: 93bc063d2bbf6880a914707a710dfed105ea36bfab97fda3d0a36bfbfb5de53d*.

### BitTorrent

***It’s an old one, and it would be updated to a new one after BT upload.*** 

[dogechain.2022-10-27T22_09_00-UTC.snapshot.torrent](http://snapshots.dogechain.dog/dogechain.2022-10-27T22_09_00-UTC.snapshot.torrent)

* *sha1sum: d32185f0d2b1be1bef63b0629a8f4e6bc38343ba*

## Usage 

Step 1: Preparation
- Make sure your hardware meets the [suggested requirement](https://docs.dogechain.dog/docs/get-started/full-node-deployment).
- A disk with enough free storage space, at least twice the size of the snapshot.
- Install `tmux` or another terminal multiplexer, if you want to run a terminal background session instead of processing in the background.

Step 2: Download
- Copy the above snapshot URL.

- Download: 
    - > wget -O dogechain.snapshot.zst "paste_snapshot_URL_here"

    - Or you can use `BitTorrent` for continuous download.

- Download takes more than 1 hours,

    - You can run the following command in the background: 
    - > nohup wget -O dogechain.snapshot.zst "paste_snapshot_URL_here" &
    - Or you can put it in a `tmux` background terminal session which can be detached and reattached later.

- Check that its `sha256sum` is consistent with the official one.
    - > sha256sum dogechain.snapshot.zst > dogechain.snapshot.zst.sha256sum
    - It will take minutes to finish checksum calculating.


Step 3: Decompression

- Decompression:

    -  > zstd -d --long=31 dogechain.snapshot.zst -c | tar -xvf - -C paste_your_target_dir_here
- Decompression takes more than 10 minutes,

    - You can run the following command in the background: 
    - > mkdir -p paste_your_target_dir_here
        >
        > nohup bash -c 'zstd -d --long=31 dogechain.snapshot.zst -c | tar -xvf - -C paste_your_target_dir_here' > dogechain-decompression.log 2>&1 &
    - Or you can put it in a `tmux` background terminal session which can be detached and reattached later.

Step 4: Replacing Data

- First, stop the running `dogechain` client via `kill {pid}`, and make sure the client is down.

- It is recommended to back up the original data:

> Dogechain_DataDir="paste_your_dogechain_data_dir_here"
>
> mv "${Dogechain_DataDir}/blockchain" "${Dogechain_DataDir}/blockchain_backup"
>
> mv "${Dogechain_DataDir}/consensus/metadata" "${Dogechain_DataDir}/consensus/metadata_backup"
>
> mv "${Dogechain_DataDir}/consensus/snapshots" "${Dogechain_DataDir}/consensus/snapshots_backup"
>
> mv "${Dogechain_DataDir}/trie" "${Dogechain_DataDir}/trie_backup"

- Replace the data:

> Dogechain_DataDir="paste_your_dogechain_data_dir_here"
>
> mv blockchain ${Dogechain_DataDir}/blockchain
>
> mv consensus/metadata ${Dogechain_DataDir}/consensus/metadata
>
> mv consensus/snapshots ${Dogechain_DataDir}/consensus/snapshots
>
> mv trie ${Dogechain_DataDir}/trie

- Start the `dogechain` client again and check the logs.
