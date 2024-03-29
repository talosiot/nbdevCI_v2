Based on https://github.com/talosiot-will/nbdevCI

# nbdevCI
> CI tools for nbdev projects

## Use
Use in a `.github/workflows` file.

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - uses: actions/setup-python@v2
    - name: Install SSH key
      uses: shimataro/ssh-key-action@v2.3.0
      with:
        key: ${{ secrets.KEY_GITHUB }}
        name: github_actions
        known_hosts: "|1|fYp42QmwYvgxeEflrhYNydeTiuo=|dA//p01ypTTLaNaYodiONRjAhmg= ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAq2A7hRGmdnm9tUDbO9IDSwBK6TbQa+PXYPCPy6rbTrTtw7PHkccKrpp0yVhp5HdEIcKr6pLlVDBfOLX9QUsyCOV0wzfjIJNlGEYsdlLJizHhbn2mUjvSAHQqZETYP81eFzLQNnPHt4EVVUh7VfDESU84KezmD5QlWpXLmvU31/yMf+Se8xhHTvKSCZIFImWwoG6mbUoWf9nzpIoaSjB+weqqUUmpaaasXVal72J+UX2B+2RPW3RcT0eOzQgqlJL3RKrTJvdsjE3JEAvGq3lGHSZXy28G3skua2SmVi/w4yCE6gbODqnTWlg7+wC604ydGXA8VJiS5ap43JXiUFFAaQ=="
        config: |
            Host github.com
                HostName github.com
                User git
                IdentityFile ~/.ssh/github_actions
                StrictHostKeyChecking no
    - uses: talosiot-will/nbdevCI@v1
```

This will attempt to clone private repos with a github ssh key.  If you don't need private repos you can skip the ssh key step.

Technically `StrictHostKeyChecking no` opens me up for a man-in-the-middle attack.  Since the man would have to be in the middle of github and github, I think the risk is low.

## Notes
- Dont use this to distribute docker images to untrusted people.  It may be possible to get your keys from the docker layers.  I have not tested this attack.
