# kdeconnect-sms

Simple bash script to send text messages from the command line with the `kdeconnect-cli` command. Read more about KDE connect here: https://play.google.com/store/apps/details?id=org.kde.kdeconnect_tp&hl=en.

The incentive behind the creation of this script is to provide simple address book functionality with bash autocompletion for sending text messages from the command line. Therefore, one does not have to lookup for a phone number manually and type it each time a message needs to be sent.

I did not bother about fancy integrations with Google contact lists or Akonadi stuff from the KDE community. This script intends to be simple as all I need is a simple name/phone address book that can be maintained in a simple text file. The most important feature that this script provides is the address book and the bash autocompletion functionality based on the address book. Other than that, it is a simple wrapper of the `kdeconnect-cli` command.

## Installation

Download the script from github and create a symbolic link in a folder available in your `$PATH` environment variable:

```bash
cd /opt
sudo git clone https://github.com/cyberang3l/kdeconnect-sms.git
sudo ln -s /opt/kdeconnect-sms/kdeconnect-sms /usr/local/bin/kdeconnect-sms
```
> **TIP**: If you think the `kdeconnect-sms` command name feels too long, you can alway rename the symlink to whatever you like. The bash completion code always works as it is based on the basename of the command. I personally rename it to `sms` by changing the `ln -s` command from the previous code box to:
>
> `sudo ln -s /opt/kdeconnect-sms/kdeconnect-sms /usr/local/bin/sms`

Create a sample `.addressBook` file in you home directory and edit as needed:

```bash
cat << EOF > ~/.addressBook
# Add the unique hex ID of the mobile device that you want to use
# for sending text messages in the variable "phoneIdToUse" below.
#
# You can get a list of all kde connect paired phone ids with the
# command "kdeconnect-cli -l".
#
phoneIdToUse=0123456789abcdef

# Rules for the addressBook:
#  1. One Name/phone per line.
#  2. The Name and phone must be enclosed in double quotes.
#  3. The name/phone must be separated by one or more whitespace characters (tabs or spaces).
#  4. Do not use the '|' character in your names or phones.
#
addressBook=(
	"John Doe"	"+4712345678"
	"Jane Doe"	"+306971234567"
)
EOF
```
Enable bash-completion for the script:

```bash
sed -i -n -e '/^source.*kdeconnect-sms/!p' -e '$a source <(kdeconnect-sms --bash-completion)' ~/.bashrc
```
> **NOTE**: If you have used a different name for the kdeconnect-sms command, you should replace the occurrences of the `kdeconnect-sms` in the previous sed command to whatever name you have used.

## Usage

```bash
$ kdeconnect-sms --help
Usage: kdeconnect-sms [OPTIONS] --send-to <phoneNumber> "text-message"
Uses kdeconnect-cli to send an sms to a phone number
listed in a user-maintained address book.

 -a, --address-book           Prints the address book nicely formatted.
 -b, --bash-completion        Prints the code for bash-completion.
                               add this line at the end of your
                               .bashrc file:
                                       source <(kdeconnect-sms -b)
 -s, --send-to PHONE_NUMBER   The phone number to send the SMS to.
                               This is a mandatory option.
 -h, --help                   Prints this help message
```

## Demo

![demo-bash-completion](https://user-images.githubusercontent.com/5658474/39655553-9c385e70-4ffa-11e8-937e-77473f322cb2.gif)
