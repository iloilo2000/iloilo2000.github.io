# Win-KeX - GUI for Kali@WSL

## Doksik

https://www.kali.org/docs/wsl/win-kex/

## Install

- Install Kali in WSL
- Install Win-KeX
  - Sudo apt update
  - Sudo apt install -y kali-win-kex

## Run Win-KeX

- Start Terminal
- New window: Kali
- Run win-kex (kali terminal ablakból)
  - Window Mode (VNC van mögötte - F8 display properties)
		`Kex --win -s`
  - Enhanced Session Mode (RDP-t használ, nagy felbontás)
		`Kex --esm --ip -s`
  - Seamless Mode (integrálódik mint a remoteapp)
		`Kex --sl -s`

Window mód a legjobb, de érdemes <F8> után kijönni full screen-ből.
