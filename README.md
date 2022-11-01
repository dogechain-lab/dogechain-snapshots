# dogechain-snapshots

## Database:

The current database is for [dogechain v1.2.0](https://github.com/dogechain-lab/dogechain/releases/tag/v1.2.0).

It is backward compatible with [dogechain v1.1.4](https://github.com/dogechain-lab/dogechain/releases/tag/v1.1.4), but is not recommended due to slow recovery.

### Endpoint

[dogechain.2022-10-27T22_09_00-UTC.snapshot.zst](http://snapshots.dogechain.dog/dogechain.2022-10-27T22_09_00-UTC.snapshot.zst)

* *file size: 88.5GB*.
* *sha1sum: d32185f0d2b1be1bef63b0629a8f4e6bc38343ba*.

## Usage 

Step 1: Preparation
- Make sure your hardware meets the [suggested requirement](https://docs.dogechain.dog/docs/get-started/full-node-deployment).
- A disk with enough free storage space, at least twice the size of the snapshot.
- Install `tmux` or another terminal multiplexer, if you want to run a terminal background session instead of processing in the background.

Step 2: Download
- Copy the above snapshot URL.
- Download: 
    - > wget -O dogechain.snapshot.zst "<paste snapshot URL here>"

- Download takes more than 1 hours,
    - You can run the following command in the background: 
    - > nohup wget -O dogechain.snapshot.zst "<paste snapshot URL here>" &
    - Or you can put it in a `tmux` background terminal session which can be detached and reattached later.
- Check that its `sha1sum` is consistent with the official one.
    - > sha1sum dogechain.snapshot.zst > dogechain.snapshot.zst.sha1sum
    - It will take minutes to finish checksum calculating.


Step 3: Decompression

- Decompression:

    -  > zstd -d --long=31 dogechain.snapshot.zst -c | tar -xvf - -C <paste your target dir here>
- Decompression takes more than 10 minutes,

    - You can run the following command in the background: 
    - > nohup zstd -d --long=31 dogechain.snapshot.zst -c | tar -xvf - -C <paste your target dir here>  > dogechain-decompression.log 2>&1 &
    - Or you can put it in a `tmux` background terminal session which can be detached and reattached later.

Step 4: Replacing Data

- First, stop the running `dogechain` client via `kill {pid}`, and make sure the client is down.

- It is recommended to back up the original data:

> Dogechain_DataDir="<paste your dogechain data dir here>"
> mv ${Dogechain_DataDir}/blockchain ${Dogechain_DataDir}/blockchain_backup
>
> mv ${Dogechain_DataDir}/consensus/metadata ${Dogechain_DataDir}/consensus/metadata_backup
>
> mv ${Dogechain_DataDir}/consensus/snapshots ${Dogechain_DataDir}/consensus/snapshots_backup
>
> mv ${Dogechain_DataDir}/trie ${Dogechain_DataDir}/trie_backup

- Replace the data:

> Dogechain_DataDir="<paste your dogechain data dir here>”
>
> mv blockchain ${Dogechain_DataDir}/blockchain
>
> mv consensus/metadata ${Dogechain_DataDir}/consensus/metadata
>
> mv consensus/snapshots ${Dogechain_DataDir}/consensus/snapshots
>
> mv trie ${Dogechain_DataDir}/trie

- Start the `dogechain` client again and check the logs.
