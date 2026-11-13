Excited to share that I contributed to my first open source project - I created a fix to ROCm/iris that caches a GPU lookup separately for each device.

I added a test to make sure switching the active GPU still returns the right result. A reviewer caught a gap in my first tests: they did not prove the default path followed a device switch. I added a regression test for switching between devices, then the reviewer validated it on AMD hardware.

The pull request merged on November 13. I learned to test what could go wrong with a cache—not just whether repeated calls get faster—and to be clear about which results I measured myself.

https://github.com/ROCm/iris/pull/638
