/**
 * 
 */
package com.bfd.search.jiebaAnalyzer;

import java.util.ArrayList;
import java.util.List;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

import com.huaban.analysis.jieba.JiebaSegmenter;
import com.huaban.analysis.jieba.SegToken;
import com.huaban.analysis.jieba.viterbi.FinalSeg;

/**
 * @class FinalSegTest
 * @package com.bfd.search.jiebaAnalyzer
 * @description TODO
 * @author super(weiguo.gao@baifendian.com)
 * @date 2014-9-26
 *
 */
public class FinalSegTest {
	
	public static void main(String[] args) {
		 String text = "我买一个《 百家姓 》,又买了一个本《 三字 经》，还买了一本《春 》刘翔和姚明一起去看范冰冰的《苹果》，大大获赞";
		 System.out.println(text.indexOf("春"));
		    Pattern p=  Pattern.compile("《[^》《]+》");
		    Matcher m=p.matcher(text);  
		    while(m.find()){  
		    	SegToken token = extract(text,m);
		    	System.out.println(token.token.substring(token.startOffset,token.endOffset));
		    }
//		testjieba();
	}

	/**
	 * @param text
	 * @param m
	 */
	private static SegToken extract(String text, Matcher m) {
		int start = m.start()+1,end=m.end()-2;
		for(int st = start,e = end;st<=e;st++){
			System.out.println(st+":"+text.charAt(st));
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

	private static void testjieba() {
		FinalSeg finalSeg = FinalSeg.getInstance();
		List<String> rs = new ArrayList<String>();
		String contents = "孙膑曾与庞涓为同窗，二人一起拜师学习兵法。庞涓后来出仕魏国，担任了魏惠王的将军，但是他认为自己的才能比不上孙膑，于是暗地派人将孙膑请到魏国加以监视。孙膑到魏国后，庞涓嫉妒他的才能，于是捏造罪名将孙膑处以膑刑和黥刑，砍去了孙膑的双足并在他脸上刺字，想使他埋没于世不为人知。当齐国使者出使至魏国首都大梁（今河南省开封市）时，孙膑以刑徒的身份秘密拜见齐国使者，用言辞打动了他。齐国使者觉得孙膑不同凡响，于是偷偷地用车将他载回齐国。逃奔到齐国的孙膑得到了田忌的赏识，于是他寄居于田忌门下担任门客。";
		
		finalSeg.viterbi(contents, rs);
		System.out.println(rs);
		JiebaSegmenter seg = new JiebaSegmenter();
		System.out.println(TokensToString(seg.process(contents, JiebaSegmenter.SegMode.SEARCH)));
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

}
