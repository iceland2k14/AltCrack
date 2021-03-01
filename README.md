# AltCrack

A tool for brute-forcing Bitcoin and other Altcoin Forks of it private keys. The main purpose of this project is to contribute to the effort of solving the [Bitcoin puzzle transaction](https://blockchain.info/tx/08389f34c98c606322740c0be6a7125d9860bb8d5cb182c02f98461e5fa6cd15): A transaction with 32 addresses that become increasingly difficult to crack.

# Modification
The options to directly search for hash160 file was added (instead of btc legacy address on original bitcrack) in a hope to be a faster alternative to Brainflayer. Since many forks or bitcoin uses the same ecdsa math from privatekey to publickey, and only differ in the address encoding. It is better to change the addresses back into the RipeMd hash 160 format which is 40 characters in hex. 

This can be done for all the coins [ Legacy BTC, Bech32 BTC, LTC, Zcash t1, Zcash t3, DOGE, XRP, DASH, BCH and many more coins]
Once all the coins have been converted into the equivalent hash160 format. The file can be given input to this program and can be search in 1 go without any performance drop.

### Note: 
      (1)  Only search from file using -i file. Don't try directly on command line with 1 value.
      (2)  If you found the privatekey, Ignore the displayed address, compression flag and displayed public key. 
      (3)  Just grab the PrivateKey. That's all is needed. Enjoy :)

### Using AltCrack

#### Usage


Use `cualtCrack.exe` for CUDA devices and `clAltCrack.exe` for OpenCL devices.

```
=================== Modified by IceLand ===================
This program searches for 20 byte HASH160 of any Altcoin from the given Hash160 file
=============================================================
AltCrack OPTIONS [TARGETS]
Where TARGETS is one or more Hash160

--help                  Display this message
-c, --compressed        Use compressed points
-u, --uncompressed      Use Uncompressed points
-r, --random            Use random values from keyspace
--compression  MODE     Specify compression where MODE is
                          COMPRESSED or UNCOMPRESSED or BOTH
-d, --device ID         Use device ID
-b, --blocks N          N blocks
-t, --threads N         N threads per block
-p, --points N          N points per thread
-i, --in FILE           Read Hash160 from FILE, one per line
-o, --out FILE          Write keys to FILE
-f, --follow            Follow text output
--list-devices          List available devices
--keyspace KEYSPACE     Specify the keyspace:
                          START:END
                          START:+COUNT
                          START
                          :END
                          :+COUNT
                        Where START, END, COUNT are in hex format
--stride N              Increment by N keys at a time
--share M/N             Divide the keyspace into N equal shares, process the Mth share
--continue FILE         Save/load progress from FILE

```

#### Examples

Multiple keys and multiple coins can be searched at once with minimal impact to performance. Provide the hash160 file in text form, one per line
```
xxAltCrack.exe -i hash160.txt
```

To start the search at a specific private key, use the `--keyspace` option:

```
xxAltCrack.exe --keyspace 766519C977831678F0000000000 -i hash160.txt
```

The `--keyspace` option can also be used to search a specific range:

```
xxAltCrack.exe --keyspace 80000000:ffffffff -i hash160.txt
```

To periodically save progress, the `--continue` option can be used. This is useful for recovering
after an unexpected interruption:


Use the `-b,` `-t` and `-p` options to specify the number of blocks, threads per block, and keys per thread.
```
xxAltCrack.exe -b 32 -t 256 -p 16 -i hash160.txt
```

### Choosing the right parameters for your device

GPUs have many cores. Work for the cores is divided into blocks. Each block contains threads.

There are 3 parameters that affect performance: blocks, threads per block, and keys per thread.


`blocks:` Should be a multiple of the number of compute units on the device. The default is 32.

`threads:` The number of threads in a block. This must be a multiple of 32. The default is 256.

`Keys per thread:` The number of keys each thread will process. The performance (keys per second)
increases asymptotically with this value. The default is256. Increasing this value will cause the
kernel to run longer, but more keys will be processed.


### Build dependencies

Visual Studio 2015 (if on Windows) any other version can be used also with proper change

For CUDA: CUDA Toolkit 10.1

For OpenCL: An OpenCL SDK (The CUDA toolkit contains an OpenCL SDK).


### Building in Windows

Open the Visual Studio solution.

Build the `clKeyFinder` project for an OpenCL build.

Build the `cuKeyFinder` project for a CUDA build.

Note: By default the NVIDIA OpenCL headers are used. You can set the header and library path for
OpenCL in the `BitCrack.props` property sheet.

### Building in Linux

Using `make`:

Build CUDA:
```
make BUILD_CUDA=1
```

Build OpenCL:
```
make BUILD_OPENCL=1
```

Or build both:
```
make BUILD_CUDA=1 BUILD_OPENCL=1
```

### Supporting this project

If you find this project useful and would like to support it, consider making a donation. Your support is greatly appreciated!

**IceLand **
```
BTC:	bc1q39meky2mn5qjq704zz0nnkl0v7kj4uz6r529at
ETH:	0xa74fC23f07A33B90d6848dF0bb409bEA5Ac16b28
DOGE:	D5Wh5bQMc3XVGdLbjJbGjryjNom5tZY6dD
```
**BTC Brichard **: `1LqJ9cHPKxPXDRia4tteTJdLXnisnfHsof`
