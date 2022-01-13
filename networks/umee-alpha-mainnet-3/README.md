# Umee Gravity Wars Testnet (`umee-alpha-mainnet-3`)

## Hardware Requirements
* **Minimum**
  * 4 GB RAM
  * 200 GB SSD
  * 2 vCPU
* **Recommended**
  * 16 GB RAM
  * 600 GB SSD
  * 6 vCPU

## Installation Steps
### Install `go`
#### Method 1 (recomended):
```bash
cd $HOME
wget -q -O go1.17.1.linux-amd64.tar.gz https://golang.org/dl/go1.17.linux-amd64.tar.gz
rm -rf /usr/local/go && tar -C /usr/local -xzf go1.17.1.linux-amd64.tar.gz && rm go1.17.1.linux-amd64.tar.gz
echo 'export GOROOT=/usr/local/go' >> $HOME/.bash_profile
echo 'export GOPATH=$HOME/go' >> $HOME/.bash_profile
echo 'export GO111MODULE=on' >> $HOME/.bash_profile
echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin' >> $HOME/.bash_profile && . $HOME/.bash_profile
go version
```

#### Method 2:
```bash
apt -y install snapd
snap install go --classic
echo 'export GOROOT=/usr/local/go' >> $HOME/.bash_profile
echo 'export GOPATH=$HOME/go' >> $HOME/.bash_profile
echo 'export GO111MODULE=on' >> $HOME/.bash_profile
echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin' >> $HOME/.bash_profile && . $HOME/.bash_profile
go version
```

### Install essentials:
```bash
cd $HOME
sudo apt update
sudo apt install make build-essential git jq ncdu bsdmainutils nload -y < "/dev/null"
```
### Install `umeed`:
```bash
cd $HOME
rm -r $HOME/umee
git clone https://github.com/umee-network/umee.git
cd umee
git pull
git checkout tags/v0.5.0-rc3
make build
cp $HOME/umee/build/umeed /usr/local/bin
umeed version
```
### Install `peggo`:
```bash
cd $HOME
rm -r $HOME/peggo
git clone https://github.com/umee-network/peggo.git
cd peggo
git pull
git checkout tags/v0.1.1
make install
peggo version
```
### Create keys
#### ⚠️ You can use your old keys
```bash
umeed keys add wallet
```
### Create genesis:
```bash
UMEE_INTERNAL_MONIKER=$(hostname) # or what you prefer
umeed init --overwrite $UMEE_INTERNAL_MONIKER --chain-id umee-alpha-mainnet-3
```
### Create genesis account
#### ⚠️ Do not change the initial balance, it will cost you spot in this week 5.
```bash
umeed add-genesis-account wallet 11000000uumee
```
### Create gentx
#### ⚠️ Do not modify the gentx directly, it will cost you spot in this testnet. If you need change something - create a new gentx, but do not forget to remove the old one
#### ⚠️ Do not change the stake amount, it will cost you spot in this week 5.
```bash
umeed gentx wallet 10000000uumee --chain-id=umee-alpha-mainnet-3 --moniker="$UMEE_INTERNAL_MONIKER"
```
### Submit gentx
- Fork [the testnets repo](https://github.com/umee-network/testnets) into your Github account

- Clone your repo using

  ```bash
  cd $HOME
  git clone https://github.com/<your-github-username>/testnets umee-testnets
  ```

- Copy the generated gentx json file to `$HOME/umee-testnets/networks/umee-alpha-mainnet-3/gentxs`

  ```bash
  cp $HOME/.umee/config/gentx/gentx-*.json $HOME/umee-testnets/networks/umee-alpha-mainnet-3/gentxs
  ```

- Commit and push to your repo
  ```bash
  cd $HOME/umee-testnets/networks/umee-alpha-mainnet-3/gentxs
  git add -A
  git commit -m "$UMEE_INTERNAL_MONIKER gentx"
  git push origin main
  ```
- Create a PR onto https://github.com/umee-network/testnets
