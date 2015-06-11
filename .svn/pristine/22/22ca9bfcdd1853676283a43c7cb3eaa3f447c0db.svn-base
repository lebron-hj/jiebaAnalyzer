package org.apache.solr.analysis.cn;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.Reader;
import java.util.ArrayList;
import java.util.List;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

import org.apache.commons.lang.StringUtils;
import org.apache.lucene.analysis.Tokenizer;
import org.apache.lucene.analysis.tokenattributes.CharTermAttribute;
import org.apache.lucene.analysis.tokenattributes.OffsetAttribute;
import org.apache.lucene.analysis.tokenattributes.PositionIncrementAttribute;
import org.apache.lucene.analysis.tokenattributes.TypeAttribute;
import org.apache.lucene.util.AttributeFactory;

import com.huaban.analysis.jieba.JiebaSegmenter;
import com.huaban.analysis.jieba.SegToken;

public class jiebaTokenizer extends Tokenizer {

	public jiebaTokenizer(AttributeFactory factory, Reader input, boolean needKeep, boolean extractStar, boolean needBookMark,boolean model) {
		super(factory, input);
		this.needKeep = needKeep;
		this.extractStar = extractStar;
		this.needBookMark = needBookMark;
		this.model = model;
	}

	private String[] unconsumed_tokens;
	  private int next_token = -1;
	  private int last_offset = 0;
	  private boolean needKeep;
	  private boolean extractStar;
	  private boolean needBookMark;
	  private boolean model;
	  private StringBuffer sb = new StringBuffer();
	  
      private final static String PUNCTION = "[　　\\| ,，。！？；,!\\?;]";
      private final static Pattern DOUBLE_MARK = Pattern.compile("《[^》《]+》");
      private final static Pattern SIMPLE_MARK = Pattern.compile("《[^><]+》");
	  private final CharTermAttribute termAtt = (CharTermAttribute)addAttribute(CharTermAttribute.class);
	  private final OffsetAttribute offsetAtt = (OffsetAttribute)addAttribute(OffsetAttribute.class);
	  private final TypeAttribute typeAtt = (TypeAttribute)addAttribute(TypeAttribute.class);
	  private final PositionIncrementAttribute posIncrAtt = (PositionIncrementAttribute)addAttribute(PositionIncrementAttribute.class);

	  void setAttribute(String token)
	  {
	    int pos = token.lastIndexOf('/');
	    if (pos == -1)
	    {
	      this.termAtt.copyBuffer(token.toCharArray(), 0, token.length());
	      this.offsetAtt.setOffset(this.last_offset, this.last_offset + token.length());
	      this.last_offset += token.length();
	      this.typeAtt.setType("word");
	    }
	    else {
	      this.termAtt.copyBuffer(token.toCharArray(), 0, pos);
	      this.offsetAtt.setOffset(this.last_offset, this.last_offset + pos);
	      this.last_offset += pos;
	      this.typeAtt.setType(token.substring(pos + 1));
	    }
	    this.posIncrAtt.setPositionIncrement(1);
	  }

	  public boolean incrementToken() throws IOException {
	    if ((this.next_token > 0) && (this.next_token < this.unconsumed_tokens.length))
	    {
	      setAttribute(this.unconsumed_tokens[(this.next_token++)]);
	      return true;
	    }
	    this.sb.setLength(0);
	    BufferedReader br = new BufferedReader(this.input);
	    String line;
	    while ((line = br.readLine()) != null)
	      this.sb.append(line);
	    if (this.sb.length() == 0)
	    {
	      this.next_token = -1;
	      this.last_offset = 0;
	      return false;
	    }
	    
	    List<SegToken> tokenList = new ArrayList<SegToken>();
	    if(this.needKeep){
	    	
	    	this.unconsumed_tokens =  this.sb.toString().split(PUNCTION);
	    	int l = 0;
	    	for(String token:this.unconsumed_tokens){
	    		if(StringUtils.isEmpty(token)){
	    			l++;
	    		}
	    		tokenList.add(new SegToken(token, l++, l+=token.length()-1));
	    		tokenList.addAll(JiebaHolder.instance.process(token, model?JiebaSegmenter.SegMode.INDEX:JiebaSegmenter.SegMode.SEARCH));
	    	}
	    }else{
	    	tokenList.addAll(JiebaHolder.instance.process(sb.toString(), model?JiebaSegmenter.SegMode.INDEX:JiebaSegmenter.SegMode.SEARCH));
	    }
	    if(this.needBookMark){
	    	 Matcher m=DOUBLE_MARK.matcher(sb.toString());  
			    while(m.find()){  
			    	String bm = m.group().trim();
			    	if(bm!=null){
			    		tokenList.add(new SegToken(bm,0,bm.length()));
			    		tokenList.add(new SegToken(bm.substring(1, bm.length()-1).trim(),0,bm.length()));
			    	}
			    }
	    }
	    if(this.extractStar){
	    	for(TrieNode node:TrieNode.search(sb.toString())){
	    		tokenList.add(new SegToken(node.getNode(), 0, node.getNode().length()));
	    	}
	    }
	    this.unconsumed_tokens = TokensToString(tokenList).split(", ");

	    this.next_token = 0;
	    this.last_offset = 0;
	    if (this.next_token < this.unconsumed_tokens.length)
	    {
	      setAttribute(this.unconsumed_tokens[(this.next_token++)]);
	      return true;
	    }
	    
	    return false;
	  }

		/**
		 * @param text
		 * @param m
		 */
		private static SegToken extract(String text, Matcher m) {
			int start = m.start()+1,end=m.end()-2;
			for(int st = start,e = end;st<=e;st++){
				if(text.charAt(st)!=' '&&text.charAt(st)!='	'){
					 start = st;
					 break ;
				}
			}
			for(int e = end;e>=start;e--){
				if(text.charAt(e)!=' '&&text.charAt(e)!='	'){
					 end = e+1;
					 break ;
				}
			}
			if(start ==end){
				return null;
			}
			return new SegToken(text, start, end);
		}
		
	  private static String TokensToString(List<SegToken> tokenList)
	  {
	    StringBuffer sb = new StringBuffer();
	    for (SegToken token : tokenList) {
	      if (sb.length() != 0)
	        sb.append(", ");
	      sb.append(token.token);
	    }
	    return sb.toString();
	  }

	  private static class JiebaHolder
	  {
	    static final JiebaSegmenter instance = new JiebaSegmenter();
	  }

}
