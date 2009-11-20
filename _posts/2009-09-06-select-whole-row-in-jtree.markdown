---
title: Select whole row in JTree
layout: post
categories:
 - java
 - jtree
 - style
 - swing
 - tree
---

How do you make a tree node in a JTree stretch over the whole row? A
TreeCellRenderer will not work since then the background will only
appear to the right of the node. The answer is to use a
TreeCellRenderer in combination with a TreeUI. Lets show how with an
example.

Lets start of with a main class that creates a frame and adds a JTree.

{% highlight java %}
import javax.swing.JFrame;
import javax.swing.JTree;
import javax.swing.tree.DefaultMutableTreeNode;


public class Main extends JFrame
{
  public Main()
  {
    DefaultMutableTreeNode root = new DefaultMutableTreeNode("Root");
    DefaultMutableTreeNode aa = new DefaultMutableTreeNode("Child AA");
    DefaultMutableTreeNode ab = new DefaultMutableTreeNode("Child AB");
    DefaultMutableTreeNode aba = new DefaultMutableTreeNode("Child ABA");
    DefaultMutableTreeNode abb = new DefaultMutableTreeNode("Child ABB");
    DefaultMutableTreeNode abba = new DefaultMutableTreeNode("Child ABBA");

    root.add(aa);
    root.add(ab);
    ab.add(aba);
    ab.add(abb);
    abb.add(abba);

    JTree tree = new MyTree(root);
    add(tree);

    pack();
    setVisible(true);
    setDefaultCloseOperation(EXIT_ON_CLOSE);
  }

  public static void main(String[] args)
  {
    new Main();
  }
}
{% endhighlight %}

Next, lets create a class that extend JTree.

{% highlight java %}
import java.awt.Color;
import java.awt.Dimension;

import javax.swing.JTree;
import javax.swing.tree.DefaultMutableTreeNode;


public class MyTree extends JTree
{
  public MyTree(DefaultMutableTreeNode root)
  {
    super(root);

    setPreferredSize(new Dimension(200, 300));
    setRootVisible(false);
    setRowHeight(25);
    setBackground(Color.BLUE);
    setUI(new MyBasicTreeUI());
    setCellRenderer(new MyNodeRenderer());
  }
}
{% endhighlight %}

Then at last, we create the cell renderer and the tree UI.

{% highlight java %}
import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Component;
import java.awt.Dimension;

import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JTree;
import javax.swing.tree.DefaultMutableTreeNode;
import javax.swing.tree.TreeCellRenderer;


public class MyNodeRenderer implements TreeCellRenderer
{
  @Override
  public Component getTreeCellRendererComponent(JTree tree, Object value, boolean selected, boolean expanded, boolean leaf, int row, boolean hasFocus)
  {
    JPanel panel = new JPanel();
    if(selected)
    {
      panel.setBackground(Color.RED);
    }
    else
    {
      panel.setBackground(Color.BLUE);
    }

    panel.setLayout(new BorderLayout());

    panel.setPreferredSize(new Dimension(tree.getWidth(), tree.getRowHeight()));

    String text = (String)((DefaultMutableTreeNode)value).getUserObject();

    panel.add(new JLabel(text));

    return panel;
  }
}
{% endhighlight %}

{% highlight java %}
import java.awt.Color;
import java.awt.Graphics;
import java.awt.Insets;
import java.awt.Rectangle;

import javax.swing.Icon;
import javax.swing.JComponent;
import javax.swing.plaf.basic.BasicTreeUI;
import javax.swing.tree.TreePath;


public class MyBasicTreeUI extends BasicTreeUI
{
  @Override
  public Icon getExpandedIcon()
  {
    return null;
  }

  @Override
  public Icon getCollapsedIcon()
  {
    return null;
  }

  @Override
  protected void paintVerticalLine(Graphics g, JComponent c, int x, int top, int bottom)
  {}

  @Override
  protected void paintHorizontalLine(Graphics g, JComponent c, int y, int left, int right)
  {}

  @Override
  protected void paintRow(Graphics g, Rectangle clipBounds, Insets insets, Rectangle bounds, TreePath path, int row, boolean isExpanded, boolean hasBeenExpanded, boolean isLeaf)
  {
    if(tree.isRowSelected(row))
    {
      g.setColor(Color.RED);
      g.fillRect(0, row * tree.getRowHeight(), tree.getWidth(), tree.getRowHeight());
    }

    super.paintRow(g, clipBounds, insets, bounds, path, row, isExpanded, hasBeenExpanded, isLeaf);
  }
}
{% endhighlight %}

Actually, you don't need to set the preferred size on the tree cell
renderer panel. But doing so like in this case will make the whole
panel clickable.
