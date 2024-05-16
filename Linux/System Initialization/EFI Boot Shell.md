- More in this YouTube [Video](https://www.youtube.com/watch?v=t_7gBLUa600).
- More in [[EFI-EN.pdf]].
- If your Bootloader is corrupted you will enter the Bootloader Shell :
  ![[Pasted image 20240312205852.png]]

- If you need help, enter the command `help` :
  ![[Pasted image 20240312210001.png]]

- To go up and down the screen, use the `fn Up/Down` or `Shift PgUP/PgDown`.
- To display the mapping Table, enter the command `map -r` :
  ![[Pasted image 20240312210219.png]]

- To jump to the EFI partition, enter the First Disk in the mapping table : `FS0:` :
  ![[Pasted image 20240312211255.png]]

## Run the Bootloader

- go to `EFI/boot` and run the `bootx64.efi`, this will run the bootloader :
  ![[Pasted image 20240312212129.png]]

## Changing the boot order

- To display the boot order, type the command `bcfg boot dump -b` :
  ![[Pasted image 20240312212928.png]]

- To change the boot order of `05` to be on the first order `00` : `bcfg boot mv 05 00`.

## help

- To display the `help` of a command like for example `bcfg` , you run `help bcfg` :
  ![[Pasted image 20240312215256.png]]

