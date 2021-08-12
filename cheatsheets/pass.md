---
title: Pass
category: CLI
layout:
updated:
---

Reference
---------

### Create

```bash
pass init [-p] <gpg-id>
pass git init
pass git remote add origin <your.git:repository>
pass git push -u --all
```

### Store

```bash
pass insert [-m] fsf.org/rsc
pass generate [-n] fsf.org/rsc length
```

### Retrieve

```bash
pass ls fsf.org/
pass show fsf.org/rsc
pass -c fsf.org/rsc
```

### Search

```bash
pass find fsf.org
```

### Management

```bash
pass mv fsf.org fsf.org/rsc
pass rm [-rf] fsf.org
pass cp fsf.org/rsc fsf.org/ricosc
```

```bash
pass edit fsf.org/rsc
```

### Synchronize

```bash
pass git push
pass git pull
```

## References

* <https://passwordstore.org>

---

THE END
