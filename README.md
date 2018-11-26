# Repo for project in IMT2007 - Network Security

## Introdution

This is the repository we used to centralize our project work. It contains our LaTeX report, our Packet Tracer implementation, notes, and general artifacts from our project. As we used the repository in a kind of ad-hoc way, it is a little messy.

Read [the report](./submitted_report.pdf) for a full discussion of our project.

The repository was at commit 84463ee971195dfff1ed5e04b2f90be63c521b53 when the presentation was held.

## Group members

* [Abdisalan Mohamed Hussein](https://github.com/migwa)
* [Job Nestor Bahner](https://github.com/jobnestor)
* [Johannes Borgen](https://github.com/brogen98)
* [Thomas Løkkeborg](https://github.com/tholok97)

## Mistakes in report

We've noticed some mistakes in our report after the time of submission. Rather than going back and changing the report we list them here:

1. **IPsec group 5**: We configured our IPsec tunnel to use group 5, but discovered that [best practice](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/sec_conn_vpnips/configuration/xe-3s/sec-sec-for-vpns-w-ipsec-xe-3s-book/sec-cfg-vpn-ipsec.html) is to use group 14 or higher.
1. **Misuse of the word "apostacy"**: We intended for this word to be an English translation of the Norwegian word "frafall", but discovered that it means [something different](https://www.merriam-webster.com/dictionary/apostasy).

## Useful links

* [Submitted report](./submitted_report.pdf)
* [Project presentation slides](./project_presentation_slides.pdf)
* [Our PT implementation configuration files](./Config/)
* [Our PT implementation save file](./packet_tracer_implementation.pkt)
* [Old README](./old/README_old.md)
* [README from the report template we used](./README_report.md)
* [Logical diagram made in draw.io](./images/logicalview.xml)
* [Google Drive](https://drive.google.com/drive/folders/1CDa-Lx_KdIyV04T9s0IQzolu0PB5MJk6)
* [Overleaf](https://www.overleaf.com/project/5be6ed7bb46170034da14220)
* [The LaTeX report template we used](https://github.com/tholok97/latex-report-template)
