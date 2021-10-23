#!/bin/bash

self=${0##*/}

if [[ ${#} -lt 2 ]]; then
	cat <<__usage__
USAGE
    ${self} diskimage mountpoint [options...]

DESCRIPTION
    ${self} mounts a partition found in the given image file diskimage
    at the specified path mountpoint using the default loop device.

    The partition to mount is selected via interactive prompt, and its
    file offset is computed automatically by parsing fdisk(8) output.

    diskimage must be a path to a whole disk image (e.g., .img, .iso).
    mountpoint is created if it does not exist. Remaining options are
    passed verbatim to mount(8).

NOTES
    Requires sudo permissions for mount(8).

__usage__
  exit 1
fi

set -e

img=${1}
mnt=${2}

parts=$( mktemp --tmpdir -q "${self}.XXXXXXX" )

echo "Partitions found:"
echo

fdisk -l -u=sectors "${img}" | perl -e'
use strict;
use warnings;

my ($ofile) = @ARGV;

open my $out, ">", $ofile or die "$!\n";

my ($units, $lnbrk, $parts);
my (@sizes);
while (<STDIN>) {
	$units = $1 and next 
		if /^\s*Units:\s*sectors of [^=]+=\s*(\d+)\s*bytes/;
	if (defined $units) {
		if (defined $parts) {
			if (/^\s*(\S+)\s+(\d+)/) {
				push @sizes, [$1, $2];
				printf "\t[%d] %s", scalar @sizes, $_;
				printf "\t\toffset: %d * %d = %d\n\n", $units, $2, $units * $2;
				printf $out "%d\n", $units * $2;
			}
		} else {
			$lnbrk = 1, next if /^\s*$/;
			$parts = 1, next if $lnbrk and /^Device/
		}
	}
  $lnbrk = 0;
}
' "${parts}"

count=$( wc -l "${parts}" )
count=${count% *}

unset -v choice

case ${count} in
	0)
		echo "No partitions found!"
		exit 1
		;;
	1)
		echo "Selected partition: 1"
		choice=1
		while true; do
			read -r -e -N 1 -p "Continue? [Y/n] " reply
			[[ "${reply}" =~ ^[Yy[:space:]]$ ]] && 
				break
			[[ "${reply}" =~ ^[Nn]$ ]] &&
				exit
		done
		;;
	*)
		while true; do
			read -r -e -p "Select partition: [#] " choice
			if [[ "${choice}" =~ ^[0-9]+$ ]]; then
				[[ ${choice} -gt 0 ]] && [[ ${choice} -le ${count} ]] &&
					break
			fi
			echo "invalid partition number: ${choice}"
		done
		;;
esac

offset=$( perl -ne "\$. == ${choice} and print" < "${parts}" )
rm -f "${parts}"
mkdir -p "${mnt}"

sudo mount -o loop,offset=${offset} "${@:3}" "${img}" "${mnt}"

