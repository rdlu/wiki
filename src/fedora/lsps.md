# LSP (Language servers)

Various language servers to use with Helix Editor or Neovim.

Install Helix in Fedora with:

```
sudo dnf copr enable varlad/helix
sudo dnf install -y helix
```

After installing the LSPs,
Check with Neovim Mason healthcheck with `:checkhealth`
Or with `hx --health`

## Typescript

Either make sure npm is not installing root with:

```
npm config set prefix '~/.local/'
```

Or use my dotfiles:

```
ln -s ~/.dotfiles/npmrc .npmrc
```

Then:

```
npm install -g typescript-language-server typescript
```

## DockerFile

```
npm install -g dockerfile-language-server-nodejs
```

## CPP

```
sudo dnf install clang-tools-extra
```

## Golang

```
sudo dnf install -y golang-x-tools-gopls
```

## HTML, CSS

```
npm i -g vscode-langservers-extracted
```

## Bash

```
npm i -g bash-language-server
```

## Rust

```
rustup component add rust-analyzer
```