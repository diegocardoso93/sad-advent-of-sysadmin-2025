# 🧩 Challenge: Kortenberg: Can't touch this!

| Property | Value |
| :--- | :--- |
| **Day** | 2025-12-03 |
| **Level** | Easy |
| **Type** | Fix |
| **Time to Solve** | 15 minutes |
| **Access** | Email |

## 📝 Description

Is "All I want for Christmas is you" already everywhere? A bit unrelated, someone messed up the permissions in this server, the *admin* user can't list new directories and can't write into new files. Fix the issue.

> **NOTE:** Besides solving the problem in your current admin shell session, you need to fix it permanently, as in a new login shell for user admin (like the one initiated by the scenario checker) should have the problem fixed as well.

## ✅ Test

The *admin* user in a separate Bash login session should be able to create a new directory in your `/home/admin` directory, as well as being able to create a file into this new directory and add text into the new file.

The "Check My Solution" button runs the script `/home/admin/agent/check.sh`, which you can see and execute.



<details>
<summary>Solution</summary>
```
# 1) Backup do /etc/profile antes de alterar
sudo cp /etc/profile /root/etc_profile.bak.$(date +%s)

# 2) Normalizar o umask em /etc/profile para 022 (sed cria .bak automático)
sudo sed -i.bak -E 's/^\s*umask\s+[0-7]{1,3}\s*$/umask 022/' /etc/profile

# 3) Procurar entradas de umask para inspecionar onde mais aparecia
sudo grep -R --line-number --color=never -E '^\s*umask\s+[0-7]{1,3}\s*$' /etc /etc/profile.d 2>/dev/null || true
sudo grep -nH --color=never -E '^\s*umask' /etc/profile || true

# 4) Tornar o script check.sh executável e ajustar posse
sudo chmod +x /home/admin/agent/check.sh
sudo chown admin:admin /home/admin/agent/check.sh
sudo chmod 755 /home/admin/agent/check.sh

# (tentativa segura de remover atributo imutável, se existisse)
sudo chattr -i /home/admin/agent/check.sh 2>/dev/null || true

# 5) Confirmar que o usuário admin consegue criar um diretório (teste de sessão)
sudo -iu admin bash -lc 'umask; mkdir ~/qq; ls -ld ~/qq; rmdir ~/qq'

# 6) Forçar um umask desejado para o usuário admin em futuras sessões (persistente)
echo "umask 0002" >> /home/admin/.profile
echo "umask 0002" >> /home/admin/.bashrc

# 7) Verificação final: abrir sessão de login como admin e rodar o script de checagem
sudo -iu admin
# dentro da sessão:
umask
sudo -iu admin /home/admin/agent/check.sh
# (no terminal interativo usei: sudo -iu admin /home/admin/agent/check.sh que retornou OK)
```

## Por que cada passo foi feito (explicação)

1. **Backup do `/etc/profile`** — segurança: antes de editar o arquivo global, criei um backup com timestamp.
2. **Ajuste do `umask` global** — havia uma linha `umask 777` em um backup detectado e possivelmente um `umask` inadequado. Padronizei `/etc/profile` para `umask 022` (bom padrão global: arquivos 644, diretórios 755).
3. **Inspeção** — procurei outras ocorrências de `umask` para entender se havia outras configurações conflitantes.
4. **Fix no `check.sh`** — tornei o script executável e garanti propriedade `admin:admin` e permissões 755, para que `admin` pudesse executá-lo.
5. **Teste de criação de diretório** — executei `mkdir` e `rmdir` dentro de uma sessão `admin` (com `sudo -iu admin`) para confirmar comportamento imediato.
6. **Persistência para usuário `admin`** — adicionei `umask 0002` em `/home/admin/.profile` e `/home/admin/.bashrc` para garantir que sessões futuras do `admin` (login interactivo e shells) recebam o `umask` que garante escrita por dono/grupo (útil em ambientes colaborativos). Isso evita regressões caso algum perfil global seja diferente.
7. **Verificação final** — conferi o `umask` ativo na sessão do `admin` e rodei `/home/admin/agent/check.sh`, que retornou `OK`, confirmando que o teste automático passou.

</details>
---


[⬅️ Back to Advent of Sysadmin 2025 README](../README.md)
