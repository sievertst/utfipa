# The UTIFPA package for LaTeX

This package is a UTF-compliant alternative for the TIPA package for LaTeX.

TIPA is an awesome package which produces beautiful renditions of the
International Phonetic Alphabet (IPA). Its only drawback is that it seems
to accomplish this beauty by simply using a custom font which renders random
characters as IPA symbols. This means that you are bound to the font they use;
you cannot use a custom font. Moreover, it is not possible to copy &amp; past
the IPA it produces; if you do, you get the underlying random characters, which
is gibberish when not rendered with the TIPA font.

This package tries to (mostly¹) work as a standin replacement which makes the 
opposite choice: It uses the correct Unicode codepoints for the IPA symbols,
meaning you can use whatever font you want and also copy &amp; paste the result.
On the other hand, it does not guarantee that those characters are rendered 
beautifully; you as a user are responsible to choose a font which does this.

¹ Only mostly because TIPA uses a somewhat arcane syntax: It's mostly X-SAMPA
but with some idiosyncracies. UTFIPA tries to adhere to X-SAMPA syntax wherever
possible, even where it clashes with TIPA (the only major deviation from X-SAMPA
still UTFIPA is that `\` is replaced with `*` due to `\` being a special
character in LaTeX).

## Usage

Assuming you have all the files in a location where LaTex can find them
(see [Installation](#installation)), you can use UTFIPA just like you would
expect:

```latex
\usepackage{utfipa}
```

## Installation

UTFIPA is not (yet) in any LaTeX distribution.

Nix users can add UTFIPA to the package list in their document flake like so:

```nix
{
  description = "UTFIPA example document";
  inputs = {
    nixpkgs.url = "github:NixOS/nixpkgs/nixos-24.05";
    utfipa-flake.url = "github:sievertst/utfipa";
  };
  outputs = {self, nixpkgs, utfipa-flake }:
    let
      system = "x86_64-linux"; # or whatever your system is
      pkgs = nixpkgs.legacyPackages.${system};
      utfipa = utfipa-flake.packages.${system}.default;

      tex = pkgs.texliveBasic.withPackages (tl:
      [ 
        utfipa
        # tl.otherPackage
      ]);
      # or alternatively using the old texlive.combine interface:
      # tex = pkgs.texlive.combine
      # {
      #   inherit (pkgs.texlive) scheme-basic
      #   utfipa
      #   # otherPackage
      #   ;
      # };

    in
    {
      packages.${system}.default = pkgs.stdenvNoCC.mkDerivation {
        pname = "some-document";
        version = "1.0";
        buildInputs = [ pkgs.coreutils tex ];      
        # rest of your document derivation
      };
    };
}
```

Non-Nix users need to copy `utfipa.sty` and `utfipa-symbols.sty` files to
a place where LaTeX can find them. This can simply be your document's root
directory. If you want to install UTFIPA system-wide, the suggested location
for the `sty` files is within `tex/latex/local/` in your local `texmf/`
directory (typically found within your home directory).

## Uninstallation

To uninstall UTFIPA, simply locate the `utfipa.sty` and `utfipa-symbols.sty`
files and delete them.
