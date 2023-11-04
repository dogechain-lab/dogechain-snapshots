# dogechain-snapshots

## Database:

The current database is for latest [dogechain bsc client](https://github.com/dogechain-lab/dbsc/releases/latest).

### Endpoint

(**Very Important**) The image is only worked on Dogechain DBSC client V2.

#### Pruned Ancient Database

Only works on DBSC client V2 after Hawaii Fork.

snapshot file: [dbsc.2023-11-02T00_33_31-UTC.snapshot.zst](https://snapshots.dogechaindev.com/dbsc.2023-11-02T00_33_31-UTC.snapshot.zst)

checksum file: [dbsc.2023-11-02T00_33_31-UTC.snapshot.zst.sha256sum](https://snapshots.dogechaindev.com/dbsc.2023-11-02T00_33_31-UTC.snapshot.zst.sha256sum)

* *file size: 45.7GiB*.
* *sha256sum: 681395b1da7cc7ed96a95957da0ce6a65fffb4fee984f3fb7c443effae9656f2*.

archive file directory tree structure:

```
${DBSC_DataDir}         # this is Dogechain DBSC client V2 data directory (`--datadir` parameter)
└── geth                # new dbsc data directory
    ├── chaindata
    └── triecache
```

#### Ancient Database

Only works on DBSC client V2 after Hawaii Fork.

snapshot file: [dbsc.2023-11-03T06_25_55-UTC.archive.snapshot.tar.zst](https://snapshots.dogechaindev.com/dbsc.2023-11-03T06_25_55-UTC.archive.snapshot.tar.zst)

checksum file: [dbsc.2023-11-03T06_25_55-UTC.archive.snapshot.tar.zst.sha256sum](https://snapshots.dogechaindev.com/dbsc.2023-11-03T06_25_55-UTC.archive.snapshot.tar.zst.sha256sum)

* *file size: 1.8TiB*.
* *sha256sum: 550b93025078c85f2c6043b6cc1ecf7019b797e0bf44b518c5314fd822a311bd*.

archive file directory tree structure:

```
${DBSC_DataDir}         # this is Dogechain DBSC client V2 data directory (`--datadir` parameter)
└── geth                # new dbsc data directory
    ├── chaindata
    └── triecache
```

## Usage

> *TIPS*: archive file is compressed by `zstd` and archived by `tar`, so you need to install `zstd` and `tar` first.

Step 1: Preparation
- Make sure your hardware meets the [suggested requirement](https://docs.dogechain.dog/docs/get-started/full-node-deployment).
- A disk with enough free storage space, at least twice the size of the snapshot.
- Install `tmux` or another terminal multiplexer, if you want to run a terminal background session instead of processing in the background.

Step 2: Download
- Copy the above snapshot URL.

- Download: 
    - > wget -O dbsc.snapshot.zst "paste_snapshot_URL_here"

- Download takes more than 1 hours,

    - You can run the following command in the background: 
    - > nohup wget -O dbsc.snapshot.zst "paste_snapshot_URL_here" &
    - Or you can put it in a `tmux` background terminal session which can be detached and reattached later.

- Check that its `sha256sum` is consistent with the official one.
    - > sha256sum dbsc.snapshot.zst > dbsc.snapshot.zst.sha256sum
    - It will take minutes to finish checksum calculating.


Step 3: Decompression

- Decompression:

    -  > zstd -d dbsc.snapshot.zst -c | tar -xvf - -C "paste_your_target_dir_here"
- Decompression takes more than 2 hours,

    - You can run the following command in the background: 
    - > mkdir -p paste_your_target_dir_here
        >
        > nohup bash -c 'zstd -d dbsc.snapshot.zst -c | tar -xvf - -C paste_your_target_dir_here' > dogechain-decompression.log 2>&1 &
    - Or you can put it in a `tmux` background terminal session which can be detached and reattached later.

Step 1-3 in one line:

- ***You can combine step 1 to 3 in oneline, but take care of the network interruption, otherwise you have to restart from the very beginning.***

- > wget "paste_snapshot_URL_here" -O - | zstd -d -c - | tar -xvf - -C "paste_your_target_dir_here"

Step 4: Replacing Data

- First, stop the running dbsc client if you have one by `kill {pid}`, and make sure the client has shut down.
- Consider backing up the original data: `mv ${DBSC_DataDir}/geth/chaindata ${DBSC_DataDir}/geth/chaindata_backup; mv ${DBSC_DataDir}/geth/triecache ${DBSC_DataDir}/geth/triecache_backup`.
- Replace the data: `mv server/data-seed/geth/chaindata ${DBSC_DataDir}/geth/chaindata; mv server/data-seed/geth/triecache ${DBSC_DataDir}/geth/triecache`.
- Start the dbsc client again and check the logs.
