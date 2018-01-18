## ��ʼCoding��汾��������VC���̳�

Coding����de��˵���Էֳ����漸���׶Σ�

1. ��Ŀ����
2. ���ܿ���
3. ����
4. ����

��εĿ���������Ҫ�����ڶ���solo��������󲿷֣�����Ȼ��������Զ���Э����Ҳ��ȫû�����⣨maybe����

### init-�½���Ŀ

#### ����һ���µ���Ŀ

�½���Ŀ����������˼��˵������������������Լ����ؿ��������Լ���Զ�̷���������ʹ�ô����й�ƽ̨��Github��Gitlab�������

����Ǳ�����Ŀ����ô��

```sh
git init --bare /path/to/repo.git

# or

ssh username@host git init --bare /path/to/repo.git
```

ʹ��ƽ̨�Ļ�����Ҫ��ƽ̨�����½���Ŀ��Ҳ����ʹ�ö�Ӧ�Ŀ���Api��д�ű���gitsomeΪ�������Լ򵥴���һ���µ���Ŀ��

```sh
gh create-repo repo "A new Repozzz.."
```

#### ��ʼ����ع���

������֮�󣬰���Ŀclone���������أ�

```sh
git clone username@host:/path/to/repo.git
cd $_
```

master��֧Ĭ����Ϊ��Ҫ������֧��

����������һЩ��Ŀ�ĳ�ʼ�������������а���һЩ��Ŀ�ر��ģ�README֮�ࡣ��ʵ��github�ϴ�����Ŀ�����У��ͻ�ѡ��һЩĬ�����á�

1. .gitignore - git�ύʱ�ĺ����ļ�
2. README.md - ��Ŀ�����ļ�
3. LICENSE - ������֤

�������ʹ����Ŀ���ּܣ��ͻ�򵥺ܶࡣ���翪��һ��react��Ŀ����������`create-react-app`�����ٴ�����

```sh
create-react-app foo
yarn test # may not need
cd foo
```

��Ŀ��ʼ�����֮�󣬾���һ���ύ��

```sh
git add .
git commit -m "Initial repo commit."
git push origin master
```

�ܽ�һ�����̣�

1. ��������Ŀ
2. ����Ŀclone�����ز�����Ŀ���ּܴ�����Ŀ
3. �ύ�����͵�Զ��һ��

�����2��3������Ի�������Ϊ�󲿷ֹ���������ʱ����һ�����ļ��У��������ǿ����ȴ���ͬ����Ŀ����clone��

> `git clone .` ֻ������һ�����ļ���

```sh
git init
git remote add -t origin path/to/repo.git
```

#### �̳���Ŀ���ּ���VC��������Ŀ

��Ȼ����Щ��������Ӧ����һ��������ʵ�֡�:heart_kissing:

```sh
create foo
```




### Github ������


1. ����һ�����ܷ�֧

�����µ����󣬱���һ���µ�feature����һ��bugfixʱ����Ҫ����һ���µķ�֧���·�֧��������Ҫ����һЩ����������master��������Ĺ��ܷ�֧��

2. ����ύ

����׶�һ����ǿ�ʼcoding����ɷ�֧�Ļ������������ύ����Ҳ������һ��������Ϊ�飬����һ���ύ���ύʱ��message��Ҫд�����

3. �����ϲ�����

��Ϊ��ǰ�������Ѿ�������ɣ��Ϳ��Դ���һ���ϲ�����pr�����ϲ�ʱ��Ҫ��д������Ϣ�������ɹ��󣬿�������Ա���ݴ˴ε��ύ�����۲������롣�����벻���ʻ���ִ��󣬱��˻�ָ�������һ�������޸��������ʱ�ᷴ�����ж����׶Σ����һЩ�µ��ύ����ȷ�����ͨ�������֮�󣬿��Գ��Բ�����Ԥ�������������鿴����ľ�����Ϊ�����ִ�����κβ�����ĵط������Իص���һ�׶����һЩ�µ��ύ����Ȼ��Ҳ���Է����˴��ύ��

4. �ϲ�����

������ɣ��ϲ���ǰ��֧������÷�֧��bugfix���͵ģ��������֧Ҳ����ûʲô���ˣ�ֱ��ɾ����˵��feature���͵ķ�֧�������ڻ����ϼ������ſ�����


## Awesomes

- [Understanding the GitHub Flow](https://guides.github.com/introduction/flow/)
- [Gitlab workflow an overview](https://about.gitlab.com/2016/10/25/gitlab-workflow-an-overview/)
- [Gitlab Flow](https://about.gitlab.com/2014/09/29/gitlab-flow/)
- [Comparing workflows](https://www.atlassian.com/git/tutorials/comparing-workflows)
