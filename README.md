# pytest-sb-ansible

Pytest plugins for running various ansible tests against VMs in vagrant.

## VMs / Vagrant

- `vagrant_run` params:
  - `playbook: str` - path to playbook
  - `project_dir: str` - path to the base of the collections directory
  - `vagrant_file: str` - path to Vagrantfile
- Returns a [`host` object](https://testinfra.readthedocs.io/en/latest/modules.html) from [pytest-testinfra](https://github.com/pytest-dev/pytest-testinfra) to make assertions

### Usage

```python
import pytest

import os


@pytest.fixture(scope='function', autouse=True)
def collection_path(request):
    return os.path.dirname(os.path.dirname(__file__))


def test_vagrant_run(collection_path, vagrant_run):
    host = vagrant_run(
        playbook=os.path.join(collection_path, "tests", "playbook-vms.yaml"),
        project_dir=collection_path,
        vagrant_file="Vagrant.ubuntu24",
    )

    assert host.file("/etc/testfile").is_file
```
