---
layout: post
title: "Java 中的多节点树"
categories: ["技术"]
tags: ["多节点树"]
---

* 目录
{:toc .toc}

声明：两种方案本人都尚未验证。

## 方案1

TreeNode.java

```java
import java.io.Serializable; 
import java.util.ArrayList; 
import java.util.List; 
 
public class TreeNode implements Serializable { 
    /**
     * 节点名 
     */ 
    protected String nodeName; 
    /**
     * 存放的对象 
     */ 
    protected Object obj; 
    /**
     * 父节点 
     */ 
    protected TreeNode parentNode; 
    /**
     * 孩子节点列表 
     */ 
    protected List<TreeNode> childList; 
    /**
     * 父节点ID 
     */ 
    private int parentId; 
    /**
     * 自己的ID 
     */ 
    private int selfId; 
 
    public TreeNode() { 
        initChildList(); 
    } 
 
    public TreeNode(TreeNode parentNode) { 
        this.getParentNode(); 
        initChildList(); 
    } 
 
    public TreeNode(String method) { 
        this.getParentNode(); 
        initChildList(); 
        nodeName = method; 
    } 
 
 
 
    /**
     * 是否叶子节点 
     * 
     * @return true 叶子节点；false 则非叶子节点. 
     */ 
    public boolean isLeaf() { 
        if (childList == null) { 
            return true; 
        } else { 
            if (childList.isEmpty()) { 
                return true; 
            } else { 
                return false; 
            } 
        } 
    } 
 
 
    /**
     * 插入一个孩子节点到当前节点中 
     * 
     * @param treeNode 孩子节点 
     */ 
    public void addChildNode(TreeNode treeNode) { 
        initChildList(); 
        childList.add(treeNode); 
    } 
 
    /**
     * 插入多个孩子节点到当前节点中 
     * 
     */ 
    public void addChildNodeList(ArrayList<TreeNode> treeNodes) { 
        initChildList(); 
        for (TreeNode treeNode : treeNodes) { 
            childList.add(treeNode); 
        } 
    } 
 
    /**
     * 初始化孩子节点列表 
     */ 
    public void initChildList() { 
        if (childList == null) { 
            childList = new ArrayList<>(); 
        } 
    } 
 
    /**
     * 是否有效树  [暂时没用] 
     * 
     * @return true 是 
     */ 
    public boolean isValidTree() { 
        return true; 
    } 
 
 
    /**
     * 返回当前节点的所有父辈节点集合 
     * 
     * @return 所有父辈节点的列表 
     */ 
    public List<TreeNode> getElders() { 
        List<TreeNode> elderList = new ArrayList<>(); 
        TreeNode parentNode = this.getParentNode(); 
        if (parentNode == null) { 
            return elderList; 
        } else { 
            elderList.add(parentNode); 
            elderList.addAll(parentNode.getElders()); 
            return elderList; 
        } 
    } 
 
 
    /**
     * 返回当前节点的晚辈集合 
     * 
     * @return 节点列表 
     */ 
    public List<TreeNode> getJuniors() { 
        List<TreeNode> juniorList = new ArrayList<>(); 
        List<TreeNode> childList = this.getChildList(); 
        if (childList == null) { 
            return juniorList; 
        } else { 
//            int childNumber = childList.size(); 
//            for (int i = 0; i < childNumber; i++) { 
//                TreeNode junior = childList.get(i); 
//                juniorList.add(junior); 
//                juniorList.addAll(junior.getJuniors()); 
//            } 
 
            for (TreeNode junior : childList) { 
                juniorList.add(junior); 
                juniorList.addAll(junior.getJuniors()); 
            } 
 
            return juniorList; 
        } 
    } 
 
    /**
     * 返回当前节点的孩子集合 
     * 
     * @return 孩子节点列表 
     */ 
    public List<TreeNode> getChildList() { 
        return childList; 
    } 
 
    /**
     * 设置孩子节点类别 
     * 
     * @param childList 孩子节点列表 
     */ 
    public void setChildList(List<TreeNode> childList) { 
        this.childList = childList; 
    } 
 
    /**
     * 删除节点和它下面的晚辈 
     */ 
    public void deleteNode() { 
        TreeNode parentNode = this.getParentNode(); 
        int id = this.getSelfId(); 
 
        if (parentNode != null) { 
            parentNode.deleteChildNode(id); 
        } 
    } 
 
    /**
     * 删除当前节点的某个子节点 
     * 
     * @param childId 
     */ 
    public void deleteChildNode(int childId) { 
        List<TreeNode> childList = this.getChildList(); 
        int childNumber = childList.size(); 
        for (int i = 0; i < childNumber; i++) { 
            TreeNode child = childList.get(i); 
            if (child.getSelfId() == childId) { 
                childList.remove(i); 
                return; 
            } 
        } 
    } 
 
    /**
     * 动态的插入一个新的节点到当前树中 
     * 
     * @param treeNode 节点 
     * @return true 插入成功; false 失败. 
     */ 
    public boolean insertJuniorNode(TreeNode treeNode) { 
        int juniorParentId = treeNode.getParentId(); 
        if (this.parentId == juniorParentId) { 
            addChildNode(treeNode); 
            return true; 
        } else { 
            List<TreeNode> childList = this.getChildList(); 
 
            boolean insertFlag; 
 
//            int childNumber = childList.size(); 
//            for (int i = 0; i < childNumber; i++) { 
//                TreeNode childNode = childList.get(i); 
//                insertFlag = childNode.insertJuniorNode(treeNode); 
//                if (insertFlag == true) 
//                    return true; 
//            } 
 
            for (TreeNode childNode : childList) { 
                insertFlag = childNode.insertJuniorNode(treeNode); 
                if (insertFlag) 
                    return true; 
            } 
 
            return false; 
        } 
    } 
 
 
    /**
     * 找到一颗树中某个节点 
     * 
     * @param id 节点id 
     * @return 节点 
     */ 
    public TreeNode findTreeNodeById(int id) { 
        if (this.selfId == id) 
            return this; 
        if (childList.isEmpty() || childList == null) { 
            return null; 
        } else { 
//            int childNumber = childList.size(); 
//            for (int i = 0; i < childNumber; i++) { 
//                TreeNode child = childList.get(i); 
//                TreeNode resultNode = child.findTreeNodeById(id); 
//                if (resultNode != null) { 
//                    return resultNode; 
//                } 
//            } 
 
            for (TreeNode child : childList) { 
                TreeNode resultNode = child.findTreeNodeById(id); 
                if (resultNode != null) { 
                    return resultNode; 
                } 
            } 
 
            return null; 
        } 
    } 
 
    /**
     * 遍历一棵树，层次遍历 
     */ 
    public void traverse() { 
        if (selfId < 0) 
            return; 
        print(this.selfId); 
 
        if (childList == null || childList.isEmpty()) { 
            return; 
        } 
 
//        int childNumber = childList.size(); 
//        for (int i = 0; i < childNumber; i++) { 
//            TreeNode child = childList.get(i); 
//            child.traverse(); 
//        } 
 
        for (TreeNode child : childList) { 
            child.traverse(); 
        } 
 
    } 
 
    /**
     * 打印内容 
     * 
     * @param content 内容 
     */ 
    public void print(String content) { 
        System.out.println(content); 
    } 
 
    /**
     * 打印内容 
     * 
     * @param content 内容 
     */ 
    public void print(int content) { 
        System.out.println(String.valueOf(content)); 
    } 
 
    /**
     * 获得父节点ID 
     * 
     * @return id 
     */ 
    public int getParentId() { 
        return parentId; 
    } 
 
    /**
     * 设置父节点ID 
     * 
     * @param parentId 父节点ID 
     */ 
    public void setParentId(int parentId) { 
        this.parentId = parentId; 
    } 
 
    /**
     * 获得当前节点ID 
     * 
     * @return ID 
     */ 
    public int getSelfId() { 
        return selfId; 
    } 
 
    /**
     * 设置当前节点ID 
     * 
     * @param selfId ID 
     */ 
    public void setSelfId(int selfId) { 
        this.selfId = selfId; 
    } 
 
    /**
     * 获得父节点 
     * 
     * @return 父节点 
     */ 
    public TreeNode getParentNode() { 
        return parentNode; 
    } 
 
    /**
     * 设置父节点 
     * 
     * @param parentNode 父节点 
     */ 
    public void setParentNode(TreeNode parentNode) { 
        this.parentNode = parentNode; 
    } 
 
    /**
     * 获得节点名 
     * 
     * @return 节点名 
     */ 
    public String getNodeName() { 
        return nodeName; 
    } 
 
    /**
     * 设置节点名 
     * 
     * @param nodeName 节点名 
     */ 
    public void setNodeName(String nodeName) { 
        this.nodeName = nodeName; 
    } 
 
    /**
     * 获得存储对象 
     * 
     * @return 
     */ 
    public Object getObj() { 
        return obj; 
    } 
 
    /**
     * 设置存储对象 
     * 
     * @param obj 对象 
     */ 
    public void setObj(Object obj) { 
        this.obj = obj; 
    } 
}
```

TreeHelper.java

```java
import java.util.ArrayList; 
import java.util.HashMap; 
import java.util.Iterator; 
import java.util.List; 

public class TreeHelper { 
 
    private TreeNode root; 
    private List<TreeNode> tempNodeList; 
    private boolean isValidTree = true; 
 
    public TreeHelper() { 
    } 
 
    public TreeHelper(List<TreeNode> treeNodeList) { 
        tempNodeList = treeNodeList; 
        generateTree(); 
    } 
 
    public static TreeNode getTreeNodeById(TreeNode tree, int id) { 
        if (tree == null) 
            return null; 
        TreeNode treeNode = tree.findTreeNodeById(id); 
        return treeNode; 
    } 
 
    /**
     * adapt the entities to the corresponding treeNode 
     * 
     * @param entityList list that contains the entities 
     * @return the list containg the corresponding treeNodes of the entities 
     */ 
    public static List<TreeNode> changeEnititiesToTreeNodes(List entityList) { 
        OrganizationEntity orgEntity = new OrganizationEntity(); 
        List<TreeNode> tempNodeList = new ArrayList<TreeNode>(); 
        TreeNode treeNode; 
 
        Iterator it = entityList.iterator(); 
        while (it.hasNext()) { 
            orgEntity = (OrganizationEntity) it.next(); 
            treeNode = new TreeNode(); 
            treeNode.setObj(orgEntity); 
            treeNode.setParentId(orgEntity.getParentId()); 
            treeNode.setSelfId(orgEntity.getOrgId()); 
            treeNode.setNodeName(orgEntity.getOrgName()); 
            tempNodeList.add(treeNode); 
        } 
        return tempNodeList; 
    } 
 
    /**
     * generate a tree from the given treeNode or entity list 
     */ 
    public void generateTree() { 
        HashMap nodeMap = putNodesIntoMap(); 
        putChildIntoParent(nodeMap); 
    } 
 
    /**
     * put all the treeNodes into a hash table by its id as the key 
     * 
     * @return hashmap that contains the treenodes 
     */ 
    protected HashMap putNodesIntoMap() { 
        int maxId = Integer.MAX_VALUE; 
        HashMap nodeMap = new HashMap<String, TreeNode>(); 
        Iterator it = tempNodeList.iterator(); 
        while (it.hasNext()) { 
            TreeNode treeNode = (TreeNode) it.next(); 
            int id = treeNode.getSelfId(); 
            if (id < maxId) { 
                maxId = id; 
                this.root = treeNode; 
            } 
            String keyId = String.valueOf(id); 
 
            nodeMap.put(keyId, treeNode); 
            // System.out.println("keyId: " +keyId); 
        } 
        return nodeMap; 
    } 
 
    /**
     * set the parent nodes point to the child nodes 
     * 
     * @param nodeMap a hashmap that contains all the treenodes by its id as the key 
     */ 
    protected void putChildIntoParent(HashMap nodeMap) { 
        Iterator it = nodeMap.values().iterator(); 
        while (it.hasNext()) { 
            TreeNode treeNode = (TreeNode) it.next(); 
            int parentId = treeNode.getParentId(); 
            String parentKeyId = String.valueOf(parentId); 
            if (nodeMap.containsKey(parentKeyId)) { 
                TreeNode parentNode = (TreeNode) nodeMap.get(parentKeyId); 
                if (parentNode == null) { 
                    this.isValidTree = false; 
                    return; 
                } else { 
                    parentNode.addChildNode(treeNode); 
                    // System.out.println("childId: " +treeNode.getSelfId()+" parentId: "+parentNode.getSelfId()); 
                } 
            } 
        } 
    } 
 
    /**
     * initialize the tempNodeList property 
     */ 
    protected void initTempNodeList() { 
        if (this.tempNodeList == null) { 
            this.tempNodeList = new ArrayList<TreeNode>(); 
        } 
    } 
 
    /**
     * add a tree node to the tempNodeList 
     */ 
    public void addTreeNode(TreeNode treeNode) { 
        initTempNodeList(); 
        this.tempNodeList.add(treeNode); 
    } 
 
    /**
     * insert a tree node to the tree generated already 
     * 
     * @return show the insert operation is ok or not 
     */ 
    public boolean insertTreeNode(TreeNode treeNode) { 
        boolean insertFlag = root.insertJuniorNode(treeNode); 
        return insertFlag; 
    } 
 
    public boolean isValidTree() { 
        return this.isValidTree; 
    } 
 
    public TreeNode getRoot() { 
        return root; 
    } 
 
    public void setRoot(TreeNode root) { 
        this.root = root; 
    } 
 
    public List<TreeNode> getTempNodeList() { 
        return tempNodeList; 
    } 
 
    public void setTempNodeList(List<TreeNode> tempNodeList) { 
        this.tempNodeList = tempNodeList; 
    } 
 
}
```

OrganizationEntity.java

```java
public class OrganizationEntity { 
    public int parentId; 
    public int orgId; 
    public String orgName; 
 
    public int getParentId() { 
        return parentId; 
    } 
 
    public void setParentId(int parentId) { 
        this.parentId = parentId; 
    } 
 
    public int getOrgId() { 
        return orgId; 
    } 
 
    public void setOrgId(int orgId) { 
        this.orgId = orgId; 
    } 
 
    public String getOrgName() { 
        return orgName; 
    } 
 
    public void setOrgName(String orgName) { 
        this.orgName = orgName; 
    } 
}
```

## 方案2

TreeNode.java

```java
public class TreeNode 
{
    /** 节点Id*/
    private String nodeId;
    /** 父节点Id*/
    private String parentId;
    /** 文本内容*/
    private String text;
    
    /**
     * 构造函数
     * 
     * @param nodeId 节点Id
     */
    public TreeNode(String nodeId)
    {
        this.nodeId = nodeId;
    }
    
    /**
     * 构造函数
     * 
     * @param nodeId 节点Id
     * @param parentId 父节点Id
     */
    public TreeNode(String nodeId, String parentId)
    {
        this.nodeId = nodeId;
        this.parentId = parentId;
    }

    public String getNodeId() {
        return nodeId;
    }

    public void setNodeId(String nodeId) {
        this.nodeId = nodeId;
    }

    public String getParentId() {
        return parentId;
    }

    public void setParentId(String parentId) {
        this.parentId = parentId;
    }

    public String getText() {
        return text;
    }

    public void setText(String text) {
        this.text = text;
    }
    
}
```

ManyTreeNode.java

```java
public class ManyTreeNode 
{
    /** 树节点*/
    private TreeNode data;
    /** 子树集合*/
    private List<ManyTreeNode> childList;
    
    /**
     * 构造函数
     * 
     * @param data 树节点
     */
    public ManyTreeNode(TreeNode data)
    {
        this.data = data;
        this.childList = new ArrayList<ManyTreeNode>();
    }
    
    /**
     * 构造函数
     * 
     * @param data 树节点
     * @param childList 子树集合
     */
    public ManyTreeNode(TreeNode data, List<ManyTreeNode> childList)
    {
        this.data = data;
        this.childList = childList;
    }

    public TreeNode getData() {
        return data;
    }

    public void setData(TreeNode data) {
        this.data = data;
    }

    public List<ManyTreeNode> getChildList() {
        return childList;
    }

    public void setChildList(List<ManyTreeNode> childList) {
        this.childList = childList;
    }

}
```

ManyNodeTree.java

```java
public class ManyNodeTree 
{
    /** 树根*/
    private ManyTreeNode root;
    
    /**
     * 构造函数
     */
    public ManyNodeTree()
    {
        root = new ManyTreeNode(new TreeNode("root"));
    }
    
    /**
     * 生成一颗多叉树，根节点为root
     * 
     * @param treeNodes 生成多叉树的节点集合
     * @return ManyNodeTree
     */
    public ManyNodeTree createTree(List<TreeNode> treeNodes)
    {
        if(treeNodes == null || treeNodes.size() < 0)
            return null;
        
        ManyNodeTree manyNodeTree =  new ManyNodeTree();
        
        //将所有节点添加到多叉树中
        for(TreeNode treeNode : treeNodes)
        {
            if(treeNode.getParentId().equals("root"))
            {
                //向根添加一个节点
                manyNodeTree.getRoot().getChildList().add(new ManyTreeNode(treeNode));
            }
            else
            {
                addChild(manyNodeTree.getRoot(), treeNode);
            }
        }
        
        return manyNodeTree;
    }
    
    /**
     * 向指定多叉树节点添加子节点
     * 
     * @param manyTreeNode 多叉树节点
     * @param child 节点
     */
    public void addChild(ManyTreeNode manyTreeNode, TreeNode child)
    {
        for(ManyTreeNode item : manyTreeNode.getChildList())
        {
            if(item.getData().getNodeId().equals(child.getParentId()))
            {
                //找到对应的父亲
                item.getChildList().add(new ManyTreeNode(child));
                break;
            }
            else
            {
                if(item.getChildList() != null && item.getChildList().size() > 0)
                {
                    addChild(item, child);
                }               
            }
        }
    }
    
    /**
     * 遍历多叉树 
     * 
     * @param manyTreeNode 多叉树节点
     * @return 
     */
    public String iteratorTree(ManyTreeNode manyTreeNode)
    {
        StringBuilder buffer = new StringBuilder();
        buffer.append("\n");
        
        if(manyTreeNode != null) 
        {   
            for (ManyTreeNode index : manyTreeNode.getChildList()) 
            {
                buffer.append(index.getData().getNodeId()+ ",");
                
                if (index.getChildList() != null && index.getChildList().size() > 0 ) 
                {   
                    buffer.append(iteratorTree(index));
                }
            }
        }
        
        buffer.append("\n");
        
        return buffer.toString();
    }
    
    public ManyTreeNode getRoot() {
        return root;
    }

    public void setRoot(ManyTreeNode root) {
        this.root = root;
    }
    
    public static void main(String[] args)
    {
        List<TreeNode> treeNodes = new ArrayList<TreeNode>();
            treeNodes.add(new TreeNode("系统权限管理", "root"));
            treeNodes.add(new TreeNode("用户管理", "系统权限管理"));
            treeNodes.add(new TreeNode("角色管理", "系统权限管理"));
            treeNodes.add(new TreeNode("组管理", "系统权限管理"));
            treeNodes.add(new TreeNode("用户菜单管理", "系统权限管理"));
            treeNodes.add(new TreeNode("角色菜单管理", "系统权限管理"));
            treeNodes.add(new TreeNode("用户权限管理", "系统权限管理"));
            treeNodes.add(new TreeNode("站内信", "root"));
            treeNodes.add(new TreeNode("写信", "站内信"));
            treeNodes.add(new TreeNode("收信", "站内信"));
            treeNodes.add(new TreeNode("草稿", "站内信"));
            
            ManyNodeTree tree = new ManyNodeTree();
            System.out.println(tree.iteratorTree(tree.createTree(treeNodes).getRoot()));
    }
    
}
```