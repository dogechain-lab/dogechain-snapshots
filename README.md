# dogechain-snapshots

## Database:

The current database is for latest [dogechain client](https://github.com/dogechain-lab/dogechain/releases/latest).

### Endpoint

snapshot file: [dogechain.2023-10-09T06_39_58-UTC.snapshot.zst](http://snapshots.dogechain.dog/dogechain.2023-10-09T06_39_58-UTC.snapshot.zst)

checksum file: [dogechain.2023-10-09T06_39_58-UTC.snapshot.zst.sha256sum](http://snapshots.dogechain.dog/dogechain.2023-10-09T06_39_58-UTC.snapshot.zst.sha256sum)

* *file size: 820GB*.
* *sha256sum: fe5fcbea48fe523ee8e463f214c192f3de9fefca0c72d2726eae14b74b3c8675*.

## Usage 

Step 1: Preparation
- Make sure your hardware meets the [suggested requirement](https://docs.dogechain.dog/docs/get-started/full-node-deployment).
- A disk with enough free storage space, at least twice the size of the snapshot.
- Install `tmux` or another terminal multiplexer, if you want to run a terminal background session instead of processing in the background.

Step 2: Download
- Copy the above snapshot URL.

- Download: 
    - > wget -O dogechain.snapshot.zst "paste_snapshot_URL_here"

- Download takes more than 1 hours,

    - You can run the following command in the background: 
    - > nohup wget -O dogechain.snapshot.zst "paste_snapshot_URL_here" &
    - Or you can put it in a `tmux` background terminal session which can be detached and reattached later.

- Check that its `sha256sum` is consistent with the official one.
    - > sha256sum dogechain.snapshot.zst > dogechain.snapshot.zst.sha256sum
    - It will take minutes to finish checksum calculating.


Step 3: Decompression

- Decompression:

    -  > zstd -d dogechain.snapshot.zst -c | tar -xvf - -C "paste_your_target_dir_here"
- Decompression takes more than 10 minutes,

    - You can run the following command in the background: 
    - > mkdir -p paste_your_target_dir_here
        >
        > nohup bash -c 'zstd -d dogechain.snapshot.zst -c | tar -xvf - -C paste_your_target_dir_here' > dogechain-decompression.log 2>&1 &
    - Or you can put it in a `tmux` background terminal session which can be detached and reattached later.

Step 1-3 in one line:

- ***You can combine step 1 to 3 in oneline, but take care of the network interruption, otherwise you have to restart from the very beginning.***

- > wget "paste_snapshot_URL_here" -O - | zstd -d -c - | tar -xvf - -C "paste_your_target_dir_here"

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
