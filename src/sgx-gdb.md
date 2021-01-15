
# build sgx-gdb

sgx-gdb依赖老版本的gdb. 

download gdb-9.1

```bash
wget https://ftp.gnu.org/gnu/gdb/gdb-9.1.tar.xz
extract gdb-9.1.tar.xz
cd gdb-9.1
mkdir build
cd build 
../configure --with-python=/usr/bin/python3 --prefix=/opt/gdb-9.1
make
sudo make install
```
modify `which sgx-gdb` , redirect GDB to /opt/gdb-9.1/bin/gdb

## test
```bash
cd ~/linux-sgx-sgx_2.10/SampleCode/SampleEnclave
make clean && make
sgx-gdb ./app

set debug-file-directory /usr/lib/debug
enable sgx_emmt
run
```

you should see when finished
```
  [Peak stack used]: 15 KB
  [Peak heap used]:  12 KB
  [Peak reserved memory used]:  0 KB

```
