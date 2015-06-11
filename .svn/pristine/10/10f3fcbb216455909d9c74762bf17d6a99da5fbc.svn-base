/**
 * 
 */
package org.apache.solr.analysis.cn;

import java.io.BufferedReader;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.nio.CharBuffer;
import java.nio.charset.Charset;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * @class TrieNode
 * @package org.apache.solr.handler.component.customer
 * @description TODO
 * @author super(weiguo.gao@baifendian.com)
 * @date 2014-10-24
 *
 */
public class TrieNode {
	
	private static TrieNode ROOT = new TrieNode('\0', true, new StringBuilder(2));
	
	private Map<Integer,TrieNode> children = new HashMap<Integer,TrieNode>(2);
	
	private StringBuilder charBuffer;
	
	private boolean isEnd;
	
	static{
		
		InputStream is = TrieNode.class.getResourceAsStream("mx.txt");
			BufferedReader buffer = null;
			try {
				buffer = new BufferedReader(new InputStreamReader(is, Charset.forName("utf-8")));
				String line;
				while((line=buffer.readLine())!=null){
					addWord(line.trim());
				}
			} catch (FileNotFoundException e) {
				e.printStackTrace();
			} catch (IOException e) {
				e.printStackTrace();
			} finally{
				if(buffer!=null){
					try {
						buffer.close();
					} catch (IOException e) {
						e.printStackTrace();
					}
				}
				if(is!=null){
					try {
						is.close();
					} catch (IOException e) {
						e.printStackTrace();
					}
				}
			}
	}
	
	private TrieNode(char key, boolean isEnd,StringBuilder charBuffer) {
		this.isEnd = isEnd;
		this.charBuffer = key=='\0'?charBuffer:charBuffer.append(key);
//		this.charBuffer = this.charBuffer.asReadOnlyBuffer();
	}
	
	public static void addWord(CharBuffer word){
		addWord(word.array());
	}
	
	public static void addWord(String word){
		addWord(word.toCharArray());
	}
	
	public static void addWord(char[] word){
		TrieNode trieNode = ROOT;
		for(char ch:word){
			trieNode = addChar(trieNode,ch);
		}
		if(trieNode!=ROOT)
			trieNode.isEnd = true;
	}

	/**
	 * @param trieNode
	 * @param ch
	 * @return
	 */
	private static TrieNode addChar(TrieNode trieNode, char ch) {
		TrieNode node = trieNode.children.get(Integer.valueOf(ch));
		if(node ==null ){
			node =  new TrieNode(ch,false,new StringBuilder(trieNode.charBuffer));
			trieNode.children.put((int)ch, node);
		}
		return node;
	}
	
	public static List<TrieNode> search(String words){
		return search(words.toCharArray());
	}
	
	public static List<TrieNode> search(char[] chars){
		List<TrieNode> nodes = new ArrayList<TrieNode>();
		TrieNode trieNode = ROOT;
		for(int i=0;i<chars.length;i++){
			if(trieNode.hasChild(chars[i])){
				trieNode = trieNode.children.get(Integer.valueOf(chars[i]));
				if(trieNode.isEnd){
					nodes.add(trieNode);
					i -= trieNode.charBuffer.length()-1;
				}
			}else{
				trieNode = ROOT;
			}
		}
		return nodes;
	}
		
	private boolean hasChild(char ch){
		return children.containsKey(Integer.valueOf(ch));
	}
	
	public String getNode(){
		return this.charBuffer.toString();
	}
	public static void main(String[] args) {
		for(TrieNode node :search("刘翔和姚明一起去看范冰冰的《苹果》，大大获赞")){
			System.out.println(node.charBuffer.toString());
		}
	}
}
